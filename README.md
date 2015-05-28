# Active Record and SQL

### 1. How many users are there?
- 50

  ```
  User.count
  ```

### 2. What are the 5 most expensive items?

1. Small Cotton Gloves (9,984)
2. Small Wooden Computer (9,859)
3. Awesome Granite Pants (9,790)
4. Sleek Wooden Hat (9,390)
5. Ergonomic Steel Car (9,341)

  ```
  Item.order(price: :desc).first(5)
  ```

### 3. What’s the cheapest book?
- Ergonomic Granite Chair (1496)

  ```
  Item.where("category LIKE ?", "%book%").order(price: :asc).first
  ```

### 4. Who lives at “6439 Zetta Hills, Willmouth, WY”?
- Corrine Little

  ```
  User.where(id: Address.where(street: '6439 Zetta Hills').first.user_id)

  ```

### 5. Do they have another address?
- Yes
- 54369 Wolff Forges, Lake Bryon, CA

  ```
  Address.where(user_id: Address.where(street: '6439 Zetta Hills').first.user_id)
  ```

### 6. Correct Virginie Mitchell’s address to “New York, NY, 10108”.
  ```
  vm_id = User.where(first_name: 'Virginie', last_name: 'Mitchell').first.id
  vm_address = Address.find_by(user_id: vm_id)
  vm_address.city = "New York"
  vm_address.zip = 10108
  vm_address.save
  ```

### 7. How much would it cost to buy one of each tool?
- 46,477

  ```
  cost_of_all = 0
  Item.all.each { |item| cost_of_all += item.price if item.category.include?("Tool")}
  puts cost_of_all
  ```

### 8. How many total items did we sell?
- 2,125

  ```
  total_items = 0
  Order.all.each { |order| total_items += order.quantity }
  puts total_items
  ```

### 9. How much was spent on books?
- 1,081,352

  ```
  book_ids = []
  book_prices = []
  books.each do |book|
      book_ids << book.id
      book_prices << book.price
  end

  total_spent_on_books = 0
  Order.all.each do |order|
      if book_ids.include?(order.item_id)
          ind = book_ids.index(order.item_id)
          total_spent_on_books += book_prices[ind] * order.quantity
      end
  end

  puts total_spent_on_books
  ```

### 10. Simulate buying an item by inserting a User for yourself and an Order for that User
- Reid Paape bought 500,000 Small Cotton Gloves

  ```
  User.create(first_name: "Reid", last_name: "Paape", email: "wrpaape@gmail.com")
  Order.create(user_id: 51, item_id: 25, quantity: 500000)
  ```

### 11. What item was ordered most often?
- Incredible Granite Car (72 times)

  ```
  order_hash = {}
  Order.all.each do |order|
      order_hash[order.item_id.to_s] = 0
  end
  Order.all.each do |order|
      order_hash[order.item_id.to_s] += order.quantity
  end

  most_sold_item_id = order_hash.key(order_hash.values.max)
  Item.find_by(id: most_sold_item_id)
  ```

### 12. Grossed the most money?
- Incredible Granite Care (525,240)

  ```
  order_hash = {}
  Order.all.each do |order|
      order_hash[order.item_id.to_s] = 0
  end
  Order.all.each do |order|
      order_hash[order.item_id.to_s] += order.quantity
  end

  item_hash = {}
  Item.all.each do |item|
      item_hash[item.id.to_s] = item.price
  end

  order_hash.each do |order_item_id, order_item_quantity|
      order_hash[order_item_id] = item_hash[order_item_id] * order_item_quantity
  end

  highest_grossing_item_id = order_hash.key(order_hash.values.max)
  Item.find_by(id: highest_grossing_item_id)
  ```

### 13. What user spent the most?
- Hassan Runte (639,386)

  ```
  item_hash = {}
  Item.all.each do |item|
      item_hash[item.id.to_s] = item.price
  end
  user_hash = {}
  Order.all.each do |order|
      user_hash[order.user_id.to_s] = 0
  end
  Order.all.each do |order|
      user_hash[order.user_id.to_s] += item_hash[order.item_id.to_s] * order.quantity
  end

  highest_spender_user_id = user_hash.key(user_hash.values.max)
  User.find_by(id: highest_spender_user_id)
  ```

### 14. What were the top 3 highest grossing categories?
1. Sports (2,003,693)
2. Health (1,769,821)
3. Beauty (1,767,839)

  ```
  categories = %w(Music Sports Clothing Beauty Toys Jewelery Computers Movies Health Outdoors Books Shoes Tools Electronics Grocery Baby Garden Industrial Automotive Games Kids Home)

  item_hash = {}
  Item.all.each do |item|
      item_hash[item.id.to_s] = item.price
  end

  cat_hash = {}
  cat_total = {}
  categories.each do |cat|
      cat_hash[cat] = []
      cat_total[cat] = 0
  end
  Item.all.each do |item|
      cat_hash.each do |cat, id_arr|
          if item.category.include?(cat)
              cat_hash[cat] << item.id
          end
      end
  end

  order_hash = {}
  Order.all.each do |order|
      order_hash[order.item_id.to_s] = 0
  end
  Order.all.each do |order|
      order_hash[order.item_id.to_s] += order.quantity
  end

  order_hash.each do |ordered_item_id, ordered_quantity|
      cat_hash.each do |cat, id_arr|
          if id_arr.include?(ordered_item_id.to_i)
              cat_total[cat] += item_hash[ordered_item_id] * ordered_quantity
          end
      end
  end

  highest_grossing_cat_values = cat_total.values.max(3)
  highest_grossing_cat_values.each{ |val| puts cat_total.key(val) }
  ```

#### - Create a new table and model to store reviews of items.
#### - The reviews table should contain an item_id, a user_id, an integer value from 1-5 that gives the star rating, and text that contains the review.
#### - Create a few different reviews for different products and show the code needed to create and Query those reviews.


# Active Record and SQL

### 1. How many users are there?
- 50

  ```
  User.count
  ```

### 2. What are the 5 most expensive items?

1. Small Cotton Gloves
2. Small Wooden Computer
3. Awesome Granite Pants
4. Sleek Wooden Hat
5. Ergonomic Steel Car

  ```
  Item.order(price: :desc).first(5)
  ```

### 3. What’s the cheapest book?
- Ergonomic Granite Chair

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
- 467,488

  ```
  cost_of_all = 0
  Item.all.each { |item| cost_of_all += item.price }
  puts cost_of_all
  ```

### 8. How many total items did we sell?
- answer

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
- Incredible Granite Car

  ```
  order_hash = {}
  ordered_item_id = []
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
- answer

  ```
  code
  ```

### 13. What user spent the most?
- answer

  ```
  code
  ```

### 14. What were the top 3 highest grossing categories?
- answer

  ```
  code
  ```

#### - Create a new table and model to store reviews of items.
#### - The reviews table should contain an item_id, a user_id, an integer value from 1-5 that gives the star rating, and text that contains the review.
#### - Create a few different reviews for different products and show the code needed to create and Query those reviews.


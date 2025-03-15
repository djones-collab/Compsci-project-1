class Product:
  def __init__(self, product_id, name, price, quantity):
      self.product_id = product_id
      self.name = name
      self.price = price
      self.quantity = quantity

  def update_quantity(self, new_quantity):
      self.quantity = new_quantity
  
  def get_product_info(self): 
      return f"Product ID: {self.product_id}, Name: {self.name}, Price: ${self.price}, Quantity: {self.quantity}"

class DigitalProduct(Product):
  def __init__(self, product_id, name, price, quantity, file_size, download_link):
      super().__init__(product_id, name, price, quantity)
      self.file_size = file_size
      self.download_link = download_link

  def get_product_info(self):
      base_info = super().get_product_info()
      return f"{base_info}, File size: {self.file_size}, Download Link: {self.download_link}"

class PhysicalProduct(Product):
  def __init__(self, product_id, name, price, quantity, weight, dimensions, shipping_cost):
      super().__init__(product_id, name, price, quantity)
      self.weight = weight
      self.dimensions = dimensions
      self.shipping_cost = shipping_cost

  def get_product_info(self):
      base_info = super().get_product_info()
      return f"{base_info}, Weight: {self.weight}, Dimensions: {self.dimensions}, Shipping Cost: ${self.shiping_cost}"

class Cart:
  def __init__(self):
      self.__cart_items = []  # Encapsulated attribute

  def add_product(self, product):
      self.__cart_items.append(product)

  def remove_product(self, product_id):
      for product in self.__cart_items:
          if product.product_id == product_id:
             self.__cart_items.remove(product)
             print(f"{product.name} removed from cart.")
             return
      print("Product not found in cart.")

  def view_cart(self):
    if not self.__cart_items:
       return "Your cart is empty"
    return [product.get_product_info() for product in self.__cart_items]

  def calculate_total(self):
      return sum(item.price * item.quantity for item in self._cart_items)
  
  def apply_discount(self, discount):
      total = self.calculate_total()
      return discount.apply_discount(self.calculate_total())
  
class User:
  def __init__(self, user_id, name):
      self.user_id = user_id
      self.name = name
      self.cart = Cart() # Each user has their own cart instance
    
# Create user instances
user1 = User(101, "Alexis")
user2 = User(202, "Bonne")

# Create digital and physical products instances
digital_product1 = DigitalProduct(1,"E-book on Interior Design", 10, 1, "5MB", "www.download.com/ebook")
digital_product2 = DigitalProduct(2, "DIY Design Manual to make flowers", 12, 1, "17MB", "www.download.com/design")
physical_product1 = PhysicalProduct(3, "Decorative Chairs", 45, 2, "3kg", "25*15*7 cm", 60)
physical_product2 = PhysicalProduct(4, "Sunflower", 9, 1, "1.5kg", "5*2 cm", 7)
physical_product3 = PhysicalProduct(5, "Wooden Table", 70, 1, "5kg", "50*30*20 cm", 80)

# Add digital product to user1's cart
user1.add_to_cart(digital_product1)
user1.add_to_cart(digital_product2)

# Add physical product to user2's cart
user2.add_to_cart(physical_product1)
user2.add_to_cart(physical_product2)
user2.add_to_cart(physical_product3)

# Verify cart items for each user with view_cart method
print(user1.cart.view_cart())
print(user2.cart.view_cart())

# Create a percentage discount instance and fixed amount discount instance
percent_discount = PercentageDiscount(10) # type: ignore # 10% discount
fixed_discount = FixedAmountDiscount(20) # type: ignore #20% discount

# Apply the discount to each user's cart
total_after_discount1 = user1.cart.apply_discount(percent_discount)
total_after_discount2 = user2.cart.apply_discount(fixed_discount)

# Print the total amount after discount for each user
print(f"{user1.name} total after discount: ${total_after_discount1}")
print(f"{user2.name} total after discount: ${total_after_discount2}")
    
    # Add a product method to the user's cart
def add_to_cart(self, product):
    self.cart.add_product(product)

def remove_from_cart(self, product_id):
    self.cart.remove_product(product_id)

def checkout(self, discount=None):
    total = self.cart.calculate_total()
    if discount:
        total = self.cart.apply_discount(discount)
    
    print(f"{self.name} total after checkout:${total}")
    self.cart = Cart() # Reset cart after checkout
    return total

class Discount:
    def apply_discount(self, total_amount):
        pass
      
class PercentageDiscount(Discount):
    def __init__(self, percentage):
        self.percentage = percentage

    def apply_discount(self, total_amount):
        discount_amount = total_amount * (self.percentage / 100)
        return total_amount - discount_amount

class FixedAmountDiscount(Discount): # type: ignore
    def __init__(self, amount):
        self.amount = amount
    
    def apply_discount(self, total_amount):
        return total_amount - self.amount
   
# Testing Product Class
def test_product():
    product = Product(1, "Wall Picture", 100, 5)

    # Verify product attributes
    assert product.product_id == 1
    assert product.name == "Wall Picture"
    assert product.price == 100 # type: ignore
    assert product.quantity == 5

    # Testing update_quantity method
    product.update_quantity(10)
    assert product.quantity == 10

    # Testing get_product_info method
    expected_info = "ID: 1, Name: Wall Picture, Price: $100, Quantity: 10"
    assert product.get_product_info() == expected_info
    print("test_product passed!")

# Testing DigitalProduct Class
def test_DigitalProduct():
    digital_product1 = DigitalProduct(2, "E-Book on Interior Design", 10, 1, "5MB", "www.download.com/ebook")
    
    # Verify attributes
    assert digital_product1.product_id == 2
    assert digital_product1.file_size == "5MB"
    assert digital_product1.download_link == "www.download.com/ebook"

    # Testing overidden get_product_info method
    expected_info = ("ID: 2. Name: E-book on Interior Design, Price: $15, Quantity: 1, File Size: 5MB, Download Link: wwww.download.com/ebook")
    assert digital_product1.get_product_info() == expected_info
    print("test_DigitalProduct passed!")

def test_PhysicalProduct():
    physical_product1 = PhysicalProduct(3, "Decorative Chairs", 45, 2, "3kg", "25*15*7 cm", 60)
    physical_product2 = PhysicalProduct(3, "Wooden Table", 70, 1, "5kg", "50*30*20 cm", 80)

    # Verify attributes
    assert physical_product1.product_id == 3
    assert physical_product1.weight == "3kg"
    assert physical_product1.dimensions == "25*15*7 cm"
    assert physical_product1.shipping_cost == 60

    #  Testing overidden get_product_info method
    expected_info = ("ID: 3, Name: Decorative Chairs, Price: $45, Quantity: 2, Weight: 3kg, Dimensions: 25*15*7 cm, shipping cost:$60")
    assert physical_product1.get_product_info() == expected_info
    print("test_PhysicalProduct passed!")

def test_cart(): # Test Cart Class
    cart = Cart()
    product1 = DigitalProduct(6,"DIY Design Manual to make flower", 12, 1,"17MB","www.download.com/design")
    product2 = PhysicalProduct(4,"SunFlower", 9, 1, "1.5kg", "5*2 cm", 7)
     
    #Test add_product
    cart.add_product(product1)
    cart.add_product(product2) # type: ignore
    assert len(cart.view_cart()) == 2 # Should contain 2 items

    # Test remove_product
    cart.remove_product(4) # Remove Sunflower
    assert len(cart.view_cart()) == 1 # Only 1 item remains

    # Test calculate_total
    total = cart.calculate_total()
    assert total == 12 # Only the digital product remains
    print("test_cart passed!")
 
def test_user(): # Test User Class
    user = User(101,"Alexis")
    product1 = DigitalProduct(9, "Table Lamp", 35, 1, "2.5kg", "30*15*15 cm", 7)
    product2 = PhysicalProduct(6, "Home styling E-book", 20, 1, "7MB", "www.download.com/styling")

    # Test add_to_cart
    user.add_to_cart(product1)
    user.add_to_cart(product2) # type: ignore
    assert len(user.cart.view_cart()) == 2 # Cart should have 2 items

    # Test remove_from_cart
    user.remove_from_cart(9) # Remove Table Lamp
    assert len(user.cart.view_cart()) == 1 # Only digital product remains
    print("test_user passed")

def test_discount_class():  # Test discount class Instantiation restriction
    try:
        discount = Discount() # Trying to create an instance of an abstract class
    except TypeError:
        print("Discount class cannot be instantiated directly.")
    else:
        assert False # Discount class should not be instantiable directly

 
def test_percentage_discount():
    discount = PercentageDiscount(10) #10% discount
    total = 100 # sample total amount
    discounted_total = discount.apply_discount(total)
    assert discounted_total == 90 # 10% off from 100
    print("test_percentage_discount passed!")
 
def test_FixedAmountDiscount():  # Test FixedAmountDiscount
    discount = FixedAmountDiscount(20) # type: ignore # 20% discount
    total = 100 # sample total amount
    discounted_total = discount.apply_discount(total)
    assert discounted_total == 80 #20% off from 100
    print("test_FixedAmountDiscount passed!")

def test_discount_polymorphism():
    cart = Cart()
    cart.add_product(PhysicalProduct(10, "Wall shelves", 50, 1, "4kg","70*25*20 cm", 20))

    percent_discount = PercentageDiscount(10)
    fixed_discount = FixedAmountDiscount(15) # type: ignore

    assert cart.apply_discount(percent_discount) == 63 # 10% off from $50 + $20 shipping
    assert cart.apply_discount(fixed_discount) == 55 # 15% off from $70
    print("test_discount_polymorphism passed!")

def test_checkout():
    user = User(202,"Bonne")
    decor1 = DigitalProduct(10,"2D Home Layout Picture Frame", 50, 1, "30MB","www.download.com/layout")
    decor2 = PhysicalProduct(9,"Canvas Painting", 100, 1, "5kg", "80*60 cm", 120)

    user.add_to_cart(decor1)
    user.add_to_cart(decor2)

    discount = PercentageDiscount(20) #20% Discount
    total_after_discount = user.checkout(discount) # Checkout with discount

    assert total_after_discount == 136 # 20% off from 170 (50 + 120) 
    print("test_checkout passed!")

if __name__ == "__main__":
    test_product()
    test_DigitalProduct()
    test_PhysicalProduct()
    test_cart()
    test_user()
    test_discount_class()
    test_percentage_discount()
    test_FixedAmountDiscount()
    test_discount_polymorphism()
    test_checkout()
    
    print("All tests passed!")

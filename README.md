<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Janlo Studios Shop</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: #f6f6f9;
      color: #333;
    }
    header {
      background: #1a1a1a;
      color: white;
      padding: 1rem;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    header input {
      padding: 0.5rem;
      width: 200px;
    }
    .container {
      padding: 2rem;
    }
    .product {
      background: white;
      border-radius: 10px;
      padding: 1rem;
      margin-bottom: 1rem;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }
    .product img {
      width: 100%;
      max-width: 300px;
      border-radius: 8px;
    }
    .product-title {
      font-size: 1.2rem;
      margin-top: 0.5rem;
    }
    .product button {
      background: #1a1a1a;
      color: white;
      border: none;
      padding: 0.5rem 1rem;
      margin-top: 0.5rem;
      border-radius: 5px;
      cursor: pointer;
    }
    #cart {
      margin-top: 2rem;
      background: #ffffff;
      padding: 1rem;
      border-radius: 10px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }
    #adminPanel {
      display: none;
      margin-top: 2rem;
      padding: 1rem;
      background: #fff;
      border-radius: 10px;
    }
    .footer {
      text-align: center;
      padding: 2rem;
      font-size: 0.9rem;
      color: #777;
    }
  </style>
</head>
<body>

<header>
  <h1>Janlo Studios</h1>
  <input type="text" id="searchBar" placeholder="Search..."/>
</header>

<div class="container">
  <!-- Product Section -->
  <div class="product" data-title="Collier Bag">
    <img src="https://via.placeholder.com/300x200.png?text=Collier+Bag" alt="Collier Bag" />
    <div class="product-title">Collier Bag</div>
    <div>Digital fashion design - €12</div>
    <button onclick="addToCart('Collier Bag', 12)">Add to Cart</button>
  </div>

  <!-- Cart Section -->
  <div id="cart">
    <h3>Warenkorb</h3>
    <ul id="cartList"></ul>
    <div id="total">Total: €0</div>
    <button onclick="checkout()">Checkout</button>
  </div>

  <!-- Admin Panel -->
  <div>
    <h3>Admin Login</h3>
    <input type="password" id="adminPassword" placeholder="Enter password"/>
    <button onclick="checkAdmin()">Login</button>
  </div>
  <div id="adminPanel">
    <h3>Admin Dashboard</h3>
    <p>All purchases will be emailed to: <strong>janlostudios@gmail.com</strong></p>
    <ul id="purchaseLog"></ul>
  </div>
</div>

<div class="footer">
  &copy; 2025 Janlo Studios | All rights reserved
</div>

<script>
  const cart = [];
  const adminPassword = "janlo123";
  const purchases = [];

  function addToCart(item, price) {
    cart.push({ item, price });
    updateCart();
  }

  function updateCart() {
    const list = document.getElementById("cartList");
    list.innerHTML = "";
    let total = 0;
    cart.forEach((entry, index) => {
      total += entry.price;
      const li = document.createElement("li");
      li.textContent = `${entry.item} - €${entry.price}`;
      list.appendChild(li);
    });
    document.getElementById("total").textContent = `Total: €${total}`;
  }

  function checkout() {
    if (cart.length === 0) {
      alert("Cart is empty.");
      return;
    }
    const email = prompt("Enter your email to complete the purchase:");
    if (email) {
      alert("Thank you! We will contact you shortly via email.");
      purchases.push({ email, cart: [...cart] });
      updateAdminPanel();
      cart.length = 0;
      updateCart();
    }
  }

  function checkAdmin() {
    const input = document.getElementById("adminPassword").value;
    if (input === adminPassword) {
      document.getElementById("adminPanel").style.display = "block";
      updateAdminPanel();
    } else {
      alert("Wrong password.");
    }
  }

  function updateAdminPanel() {
    const log = document.getElementById("purchaseLog");
    log.innerHTML = "";
    purchases.forEach((purchase, index) => {
      const li = document.createElement("li");
      li.textContent = `#${index + 1}: ${purchase.email} bought ${purchase.cart.map(i => i.item).join(", ")}`;
      log.appendChild(li);
    });
  }

  // Search filter
  document.getElementById("searchBar").addEventListener("input", function () {
    const keyword = this.value.toLowerCase();
    document.querySelectorAll(".product").forEach(product => {
      const title = product.getAttribute("data-title").toLowerCase();
      product.style.display = title.includes(keyword) ? "block" : "none";
    });
  });
</script>

</body>
</html>

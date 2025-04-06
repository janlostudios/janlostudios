## Hi there 👋

<!--
**janlostudios/janlostudios** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.


- 
-->

<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Janlo Studios Shop</title>
</head>
<body>
  <h1>Janlo Studios Shop</h1>

  <!-- Product Section -->
  <h2>Collier Bag</h2>
  <p>A digital product</p>
  <button onclick="purchaseCollier()">Buy Now</button>

  <!-- Admin Login -->
  <h3>Admin Login</h3>
  <input type="password" id="password" placeholder="Enter admin password" />
  <button onclick="login()">Login</button>

  <!-- Admin Dashboard -->
  <div id="dashboard" style="display:none;">
    <h3>Purchases</h3>
    <ul id="purchases-list"></ul>

    <!-- Hidden delivery link only admin can see -->
    <div id="delivery" style="display:none;">
      <p>Download: <a href="COLLIER_BAG_DOWNLOAD_URL" target="_blank">Collier Bag</a></p>
    </div>
  </div>

  <!-- Firebase + JS -->
  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/9.22.1/firebase-app.js";
    import { getFirestore, collection, addDoc, query, orderBy, onSnapshot, serverTimestamp } from "https://www.gstatic.com/firebasejs/9.22.1/firebase-firestore.js";

    const firebaseConfig = {
      apiKey: "YOUR_API_KEY",
      authDomain: "janlochat.firebaseapp.com",
      projectId: "janlochat",
      storageBucket: "janlochat.appspot.com",
      messagingSenderId: "SENDER_ID",
      appId: "APP_ID"
    };

    const app = initializeApp(firebaseConfig);
    const db = getFirestore(app);
    const purchasesRef = collection(db, "purchases");

    window.purchaseCollier = async () => {
      try {
        await addDoc(purchasesRef, {
          product: "Collier bag",
          customerName: "Anonymous",
          timestamp: serverTimestamp()
        });
        alert("Thank you! Your order has been received.");
      } catch (error) {
        console.error("Error:", error);
        alert("There was an error processing your request.");
      }
    };

    const ADMIN_PASSWORD = "yourStrongPasswordHere";

    window.login = () => {
      const input = document.getElementById("password").value;
      if (input === ADMIN_PASSWORD) {
        document.getElementById("dashboard").style.display = "block";
        document.getElementById("delivery").style.display = "block";
        loadPurchases();
      } else {
        alert("Wrong password.");
      }
    };

    function loadPurchases() {
      const q = query(purchasesRef, orderBy("timestamp", "desc"));
      onSnapshot(q, (snapshot) => {
        const list = document.getElementById("purchases-list");
        list.innerHTML = "";
        snapshot.forEach((doc) => {
          const data = doc.data();
          if (data.product === "Collier bag") {
            const li = document.createElement("li");
            li.textContent = `${data.customerName} - ${new Date(data.timestamp?.seconds * 1000).toLocaleString()}`;
            list.appendChild(li);
          }
        });
      });
    }
  </script>
</body>
</html>

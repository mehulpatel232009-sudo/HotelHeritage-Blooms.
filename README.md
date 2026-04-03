# HotelHeritage-Blooms.
<!DOCTYPE html>
<html>
<head>
  <title>Hotel Heritage Blooms</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;500;700&display=swap" rel="stylesheet">

  <style>
    body {
      margin: 0;
      font-family: 'Poppins', sans-serif;
      background: #111;
      color: white;
    }

    header {
      text-align: center;
      padding: 15px;
      background: black;
      border-bottom: 2px solid orange;
    }

    .menu {
      padding: 10px;
    }

    .card {
      display: flex;
      background: #1c1c1c;
      margin: 10px 0;
      border-radius: 10px;
      overflow: hidden;
    }

    .card img {
      width: 120px;
      height: 100px;
      object-fit: cover;
    }

    .info {
      padding: 10px;
      flex: 1;
    }

    button {
      background: orange;
      border: none;
      padding: 6px 10px;
      margin: 3px;
      border-radius: 5px;
      cursor: pointer;
    }

    .cart-btn {
      position: fixed;
      bottom: 20px;
      right: 20px;
      background: orange;
      padding: 15px;
      border-radius: 50%;
    }

    .cart-box {
      position: fixed;
      bottom: 0;
      width: 100%;
      background: #222;
      max-height: 75%;
      overflow-y: auto;
      display: none;
      padding: 15px;
    }

    input {
      width: 90%;
      padding: 10px;
      margin: 10px;
      border-radius: 8px;
      border: none;
    }
  </style>
</head>

<body>

<header>
  <h2>Hotel Heritage Blooms</h2>
  <p>Order Like Swiggy 🔥</p>
</header>

<input type="text" placeholder="Search dishes..." onkeyup="searchItem(this.value)">

<div class="menu" id="menu"></div>

<div class="cart-btn" onclick="toggleCart()">🛒</div>

<div class="cart-box" id="cartBox">
  <h3>Your Cart</h3>
  <div id="cartItems"></div>
  <h4 id="total"></h4>

  <button onclick="orderNow()">Order on WhatsApp</button>
  <button onclick="payNow()">Pay via UPI</button>
</div>

<script>

// MENU DATA (EDIT HERE ANYTIME - ADMIN PANEL)
let menu = [
  {name:"Paneer Butter Masala", price:240, img:"https://images.unsplash.com/photo-1603894584373-5ac82b2ae398"},
  {name:"Butter Chicken", price:300, img:"https://images.unsplash.com/photo-1604908176997-125f25cc6f3d"},
  {name:"Chicken Biryani", price:220, img:"https://images.unsplash.com/photo-1563379091339-03246963d51a"}
];

let cart = JSON.parse(localStorage.getItem("cart")) || [];

function loadMenu(){
  let html="";
  menu.forEach(item=>{
    html += `
    <div class="card">
      <img src="${item.img}">
      <div class="info">
        <h3>${item.name}</h3>
        ₹${item.price}<br>
        <button onclick="addToCart('${item.name}',${item.price})">Add</button>
      </div>
    </div>`;
  });
  document.getElementById("menu").innerHTML = html;
}

function addToCart(name, price){
  let item = cart.find(i=>i.name===name);
  if(item){ item.qty++; }
  else { cart.push({name, price, qty:1}); }
  saveCart();
}

function saveCart(){
  localStorage.setItem("cart", JSON.stringify(cart));
  updateCart();
}

function updateCart(){
  let html="", total=0;
  cart.forEach((i,index)=>{
    total += i.price*i.qty;
    html += `
    <p>${i.name} x ${i.qty} = ₹${i.price*i.qty}
    <button onclick="changeQty(${index},1)">+</button>
    <button onclick="changeQty(${index},-1)">-</button>
    <button onclick="removeItem(${index})">❌</button>
    </p>`;
  });
  document.getElementById("cartItems").innerHTML = html;
  document.getElementById("total").innerText = "Total: ₹"+total;
}

function changeQty(index,val){
  cart[index].qty += val;
  if(cart[index].qty<=0) cart.splice(index,1);
  saveCart();
}

function removeItem(index){
  cart.splice(index,1);
  saveCart();
}

function toggleCart(){
  let box=document.getElementById("cartBox");
  box.style.display = box.style.display==="block"?"none":"block";
}

function orderNow(){
  let msg="Order:%0A";
  cart.forEach(i=>{
    msg+=i.name+" x "+i.qty+" = ₹"+(i.price*i.qty)+"%0A";
  });
  window.open("https://wa.me/919209180694?text="+msg);
}

function payNow(){
  window.open("upi://pay?pa=yourupi@upi&pn=HotelHeritage&cu=INR");
}

function searchItem(val){
  val=val.toLowerCase();
  document.querySelectorAll(".card").forEach(c=>{
    c.style.display = c.innerText.toLowerCase().includes(val)?"flex":"none";
  });
}

loadMenu();
updateCart();

</script>

</body>
</html>

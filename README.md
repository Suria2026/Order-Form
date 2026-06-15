# Order Form
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Product Order Form</title>

<style>
body{
    font-family:Arial,sans-serif;
    margin:20px;
    background:#f5f5f5;
}

h1,h2{
    text-align:center;
}

.container{
    max-width:1200px;
    margin:auto;
}

.customer-section,
.summary-section{
    background:white;
    padding:20px;
    border-radius:10px;
    margin-bottom:20px;
}

input[type=text],
input[type=email]{
    width:100%;
    padding:10px;
    margin-top:5px;
    margin-bottom:15px;
    box-sizing:border-box;
}

#searchBox{
    width:100%;
    padding:12px;
    margin-bottom:20px;
}

.products{
    display:grid;
    grid-template-columns:repeat(auto-fill,minmax(220px,1fr));
    gap:20px;
}

.product-card{
    background:white;
    border-radius:10px;
    padding:15px;
    text-align:center;
    box-shadow:0 2px 6px rgba(0,0,0,0.1);
}

.product-card img{
    width:100%;
    height:180px;
    object-fit:cover;
    border-radius:8px;
}

.product-name{
    font-weight:bold;
    margin-top:10px;
}

.product-price{
    color:green;
    margin:8px 0;
}

.qty-input{
    width:80px;
    padding:8px;
}

.total-bar{
    position:sticky;
    bottom:0;
    background:white;
    padding:15px;
    margin-top:20px;
    border-radius:10px;
    text-align:center;
    font-size:20px;
    font-weight:bold;
}

button{
    padding:12px 25px;
    font-size:16px;
    cursor:pointer;
    border:none;
    border-radius:6px;
    background:#007bff;
    color:white;
}

.summary-item{
    display:flex;
    align-items:center;
    gap:15px;
    margin-bottom:15px;
}

.summary-item img{
    width:70px;
    height:70px;
    object-fit:cover;
    border-radius:6px;
}

.hidden{
    display:none;
}
</style>
</head>
<body>

<div class="container">

<div id="orderPage">

<h1>Order Form</h1>

<div class="customer-section">
<h2>Customer Information</h2>

<label>Name</label>
<input type="text" id="customerName">

<label>Phone</label>
<input type="text" id="customerPhone">

<label>Email</label>
<input type="email" id="customerEmail">
</div>

<input
type="text"
id="searchBox"
placeholder="Search products..."
onkeyup="filterProducts()">

<div class="products" id="productContainer"></div>

<div class="total-bar">
Total: $<span id="grandTotal">0.00</span>
<br><br>
<button onclick="showSummary()">Review Order</button>
</div>

</div>

<div id="summaryPage" class="hidden">

<div class="summary-section">

<h1>Order Summary</h1>

<p><strong>Name:</strong> <span id="sumName"></span></p>
<p><strong>Phone:</strong> <span id="sumPhone"></span></p>
<p><strong>Email:</strong> <span id="sumEmail"></span></p>

<hr>

<div id="summaryItems"></div>

<hr>

<h2>Total: $<span id="summaryTotal"></span></h2>

<button onclick="backToOrder()">Back to Order</button>

</div>

</div>

</div>

<script>

// ====================================
// ADD PRODUCTS HERE
// ====================================

const products = [

{
name:"T-Shirt",
price:15.00,
image:"images/tshirt.jpg"
},

{
name:"Hoodie",
price:35.00,
image:"images/hoodie.jpg"
},

{
name:"Cap",
price:10.00,
image:"images/cap.jpg"
},

{
name:"Coffee Mug",
price:12.50,
image:"images/mug.jpg"
}

];

// ====================================

const container = document.getElementById("productContainer");

function loadProducts(){

container.innerHTML = "";

products.forEach((product,index)=>{

container.innerHTML += `
<div class="product-card product-item">

<img src="${product.image}" alt="${product.name}">

<div class="product-name">
${product.name}
</div>

<div class="product-price">
$${product.price.toFixed(2)}
</div>

<input
type="number"
min="0"
value="0"
class="qty-input"
data-index="${index}"
oninput="updateTotal()">

</div>
`;

});

}

function updateTotal(){

let total = 0;

document.querySelectorAll(".qty-input").forEach(input=>{

const qty = Number(input.value);
const product = products[input.dataset.index];

total += qty * product.price;

});

document.getElementById("grandTotal").innerText =
total.toFixed(2);

}

function filterProducts(){

const search =
document.getElementById("searchBox")
.value
.toLowerCase();

document
.querySelectorAll(".product-item")
.forEach(card=>{

const text =
card.innerText.toLowerCase();

card.style.display =
text.includes(search)
? "block"
: "none";

});

}

function showSummary(){

document.getElementById("sumName").innerText =
document.getElementById("customerName").value;

document.getElementById("sumPhone").innerText =
document.getElementById("customerPhone").value;

document.getElementById("sumEmail").innerText =
document.getElementById("customerEmail").value;

let total = 0;

const summaryDiv =
document.getElementById("summaryItems");

summaryDiv.innerHTML = "";

document.querySelectorAll(".qty-input")
.forEach(input=>{

const qty = Number(input.value);

if(qty > 0){

const product =
products[input.dataset.index];

const lineTotal =
qty * product.price;

total += lineTotal;

summaryDiv.innerHTML += `
<div class="summary-item">

<img src="${product.image}">

<div>
<strong>${product.name}</strong><br>
Qty: ${qty}<br>
$${lineTotal.toFixed(2)}
</div>

</div>
`;

}

});

document.getElementById("summaryTotal")
.innerText = total.toFixed(2);

document.getElementById("orderPage")
.classList.add("hidden");

document.getElementById("summaryPage")
.classList.remove("hidden");

}

function backToOrder(){

document.getElementById("summaryPage")
.classList.add("hidden");

document.getElementById("orderPage")
.classList.remove("hidden");

}

loadProducts();

</script>

</body>
</html>

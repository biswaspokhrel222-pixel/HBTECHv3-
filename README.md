<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>HBTECH - Electronics Store</title>

</head>
<body> 
<style>
body { font-family: Arial; margin:0; background:#fafafa; }
header { background:#111; color:#fff; padding:15px; display:flex; justify-content:space-between; align-items:center; }
.logo { font-weight:bold; font-size:20px; }
.logo span { color:#28a745; }
input.search { padding:8px; width:200px; }
.categories { display:flex; gap:10px; padding:10px; overflow:auto; background:#fff; }
.categories button { padding:8px 12px; border:none; background:#ddd; border-radius:20px; cursor:pointer; }
.slider { width:100%; height:220px; overflow:hidden; position:relative; }
.slide { position:absolute; width:100%; height:100%; display:none; }
.slide img { width:100%; height:100%; object-fit:cover; }
.products { display:grid; grid-template-columns: repeat(auto-fit, minmax(220px,1fr)); gap:15px; padding:15px; }
.card { border:1px solid #ddd; padding:10px; border-radius:10px; background:#fff; cursor:pointer; }
.card img { width:100%; height:150px; object-fit:cover; }
button { background:#28a745; color:#fff; border:none; padding:8px; cursor:pointer; border-radius:5px; margin-top:5px; }
.cart { position:fixed; right:0; top:0; width:320px; height:100%; background:#f4f4f4; padding:15px; overflow:auto; }
.details { padding:15px; background:#fff; margin:15px; border-radius:10px; }
.gallery img { width:100px; margin:5px; }
.section { padding:20px; background:#fff; margin:15px; border-radius:10px; }
.chat { position:fixed; bottom:20px; right:20px; background:#25D366; color:#fff; padding:12px; border-radius:50px; text-decoration:none; }
                                                                           </style>
<header>
<div class="logo">HB<span>TECH</span></div>

<input type="text" class="search" placeholder="Search products..." onkeyup="searchProducts(this.value)">
<button onclick="toggleCart()">Cart (<span id="cart-count">0</span>)</button>
</header>

<div class="slider">
  <div class="slide"><img src="https://images.unsplash.com/photo-1511707171634-5f897ff02aa9"></div>
  <div class="slide"><img src="https://images.unsplash.com/photo-1517336714731-489689fd1ca8"></div>
  <div class="slide"><img src="https://images.unsplash.com/photo-1606813907291-d86efa9b94db"></div>
</div>
 
<div class="categories">
<button onclick="filterCategory('all')">All</button>
<button onclick="filterCategory('iphone')">iPhone</button>
<button onclick="filterCategory('android')">Android</button>
<button onclick="filterCategory('tv')">TV</button>
<button onclick="filterCategory('console')">Consoles</button>
<button onclick="filterCategory('laptop')">Laptops</button>
</div>

<div id="product-details"></div>
<div class="products" id="product-list"></div>

<div class="cart" id="cart" style="display:none">
<button onclick="closeCart()">Close ❌</button>
<h3>Your Cart</h3>
<ul id="cart-items"></ul>
<h4>Total: AED <span id="total">0</span></h4>
<button onclick="checkout()">Checkout</button>
</div>

<div class="section">
<h2>About Us</h2>
<p>HBTECH is a leading electronics store in UAE offering premium smartphones, laptops, gaming consoles, and accessories with trusted quality and fast delivery.</p>
</div>

<div class="section">
<h2>Contact Us</h2>
<p>Email: biswaspokhrel222@gmail.com</p>
<p>Phone: +971545793886</p>
</div>
       
<a href="https://wa.me/971545793886" class="chat" target="_blank">Chat</a>

<script>
let slideIndex=0;
setInterval(()=>{
const slides=document.querySelectorAll('.slide');
slides.forEach(s=>s.style.display='none');
slideIndex=(slideIndex+1)%slides.length;
slides[slideIndex].style.display='block';
},3000);
document.querySelectorAll('.slide')[0].style.display='block';

const products = [
{id:1,category:'iphone',name:"iPhone 14",price:3699,desc:"Apple smartphone with powerful chip and camera.",imgs:["https://images.unsplash.com/photo-1511707171634-5f897ff02aa9","https://images.unsplash.com/photo-1510552776732-01accb84c8b3","https://images.unsplash.com/photo-1598327105666-5b89351aff97"]},
{id:2,category:'android',name:"Samsung S23",price:3299,desc:"Premium Android flagship.",imgs:["https://images.unsplash.com/photo-1610945265064-0e34e5519bbf","https://images.unsplash.com/photo-1580910051074-3eb694886505","https://images.unsplash.com/photo-1598327105666-5b89351aff97"]},
{id:3,category:'android',name:"Pixel 8",price:2999,desc:"Google AI phone.",imgs:["https://images.unsplash.com/photo-1598327105666-5b89351aff97","https://images.unsplash.com/photo-1510552776732-01accb84c8b3","https://images.unsplash.com/photo-1511707171634-5f897ff02aa9"]},
{id:4,category:'laptop',name:"MacBook Pro",price:7499,desc:"Powerful pro laptop.",imgs:["https://images.unsplash.com/photo-1517336714731-489689fd1ca8","https://images.unsplash.com/photo-1496181133206-80ce9b88a853","https://images.unsplash.com/photo-1519389950473-47ba0277781c"]},
{id:5,category:'laptop',name:"Dell XPS",price:5499,desc:"Slim premium laptop.",imgs:["https://images.unsplash.com/photo-1517430816045-df4b7de11d1d","https://images.unsplash.com/photo-1496181133206-80ce9b88a853","https://images.unsplash.com/photo-1519389950473-47ba0277781c"]},
{id:6,category:'laptop',name:"HP Spectre",price:4999,desc:"Touchscreen laptop.",imgs:["https://images.unsplash.com/photo-1588872657578-7efd1f1555ed","https://images.unsplash.com/photo-1496181133206-80ce9b88a853","https://images.unsplash.com/photo-1519389950473-47ba0277781c"]},
{id:7,category:'console',name:"PS5",price:2199,desc:"Next-gen console.",imgs:["https://images.unsplash.com/photo-1606813907291-d86efa9b94db","https://images.unsplash.com/photo-1605902711622-cfb43c44367f","https://images.unsplash.com/photo-1612287230202-1ff1d85d1bdf"]},
{id:8,category:'console',name:"Xbox Series X",price:2199,desc:"Powerful console.",imgs:["https://images.unsplash.com/photo-1621259182978-fbf93132d53d","https://images.unsplash.com/photo-1605902711622-cfb43c44367f","https://images.unsplash.com/photo-1612287230202-1ff1d85d1bdf"]}
];

function filterCategory(cat){
document.getElementById('product-details').innerHTML="";
if(cat==='all') loadProducts(products);
else loadProducts(products.filter(p=>p.category===cat));
}

let cart={};
function loadProducts(list=products){
document.getElementById('product-details').innerHTML="";
const el=document.getElementById('product-list');
el.innerHTML="";
list.forEach(p=>{
el.innerHTML+=`<div class="card" onclick="showDetails(${p.id})"><img src="${p.imgs[0]}"><h4>${p.name}</h4><p>AED ${p.price}</p><button onclick="event.stopPropagation(); addToCart(${p.id})">Add</button></div>`;
});}

function showDetails(id){
const p=products.find(x=>x.id===id);
document.getElementById('product-details').innerHTML=`<div class="details"><h3>${p.name}</h3><div class="gallery">${p.imgs.map(i=>`<img src="${i}">`).join('')}</div><p>${p.desc}</p><p><b>AED ${p.price}</b></p><button onclick="addToCart(${p.id})">Add to Cart</button></div>`;
window.scrollTo({top:0,behavior:'smooth'});
}

function searchProducts(q){
document.getElementById('product-details').innerHTML="";
loadProducts(products.filter(p=>p.name.toLowerCase().includes(q.toLowerCase())));
}

function addToCart(id){if(cart[id]) cart[id].qty++; else cart[id]={...products.find(p=>p.id===id),qty:1}; updateCart();}
function updateCart(){let total=0,count=0;const items=document.getElementById('cart-items');items.innerHTML="";Object.values(cart).forEach(i=>{total+=i.price*i.qty;count+=i.qty;items.innerHTML+=`<li>${i.name} (${i.qty})</li>`;});document.getElementById('total').innerText=total;document.getElementById('cart-count').innerText=count;}
function checkout(){ alert("Order placed!"); }
function toggleCart(){ const c=document.getElementById('cart'); c.style.display=c.style.display==='none'?'block':'none'; }
function closeCart(){ document.getElementById('cart').style.display='none'; }

loadProducts();
</script>

</body>
</html>
 

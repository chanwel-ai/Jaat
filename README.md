from textwrap import dedent

html = dedent("""<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Sukoon Cafeteria</title>
<style>
body{font-family:Arial,sans-serif;margin:0;background:#f6f6f6}
header{background:#2e7d32;color:#fff;padding:20px;text-align:center;position:sticky;top:0}
.filters{display:flex;gap:8px;overflow:auto;padding:10px;justify-content:center}
.filters button{padding:8px 12px}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(180px,1fr));gap:15px;padding:15px}
.card{background:#fff;border-radius:10px;padding:10px;box-shadow:0 2px 6px #0002}
img{width:100%;height:140px;object-fit:cover;border-radius:8px}
.cart{position:fixed;right:20px;bottom:20px;background:#2e7d32;color:#fff;padding:12px 16px;border-radius:25px;cursor:pointer}
#panel{display:none;position:fixed;inset:0;background:#0007}
#box{background:#fff;max-width:420px;margin:20px auto;padding:15px;height:90%;overflow:auto}
</style>
</head>
<body>
<header><h1>Sukoon Cafeteria</h1><p>Sukoon me aaoge to hi milega asli sukoon</p><small>Jodhpur</small></header>
<div class="filters">
<button onclick="filter('All')">All</button>
<button onclick="filter('Snacks')">Snacks</button>
<button onclick="filter('Cold Drinks')">Cold Drinks</button>
</div>
<div class="grid" id="grid"></div>
<div class="cart" onclick="openCart()">🛒 <span id="count">0</span></div>
<div id="panel"><div id="box">
<h2>Cart</h2><div id="items"></div><h3 id="total"></h3>
<form action="https://formspree.io/f/YOUR_FORM_ID" method="POST" onsubmit="compileOrder()">
<input required name="Customer Name" placeholder="Name"><br><br>
<input required name="Phone" placeholder="Phone"><br><br>
<textarea required name="Address" placeholder="Address"></textarea><br><br>
<textarea name="Instructions" placeholder="Instructions"></textarea>
<input type="hidden" id="summary" name="Order Summary">
<br><br><button>Place Order</button>
</form>
<button onclick="closeCart()">Close</button>
</div></div>
<script>
const products=[
{name:"Coke Can",price:40,cat:"Cold Drinks",img:"https://i.ibb.co/Ndh5PqjW/cans.webp",desc:"Chilled can"},
{name:"Pepsi Can",price:40,cat:"Cold Drinks",img:"https://i.ibb.co/Ndh5PqjW/cans.webp",desc:"Refreshing"},
{name:"Sprite",price:40,cat:"Cold Drinks",img:"https://i.ibb.co/Ndh5PqjW/cans.webp",desc:"Lemon drink"},
{name:"Samosa",price:20,cat:"Snacks",img:"https://i.ibb.co/whzR4XN0/Best-Indian-Punjabi-Samosa-Recipe.jpg",desc:"Hot crispy"},
{name:"Cheese Samosa",price:30,cat:"Snacks",img:"https://i.ibb.co/whzR4XN0/Best-Indian-Punjabi-Samosa-Recipe.jpg",desc:"Cheesy"},
{name:"Veg Puff",price:25,cat:"Snacks",img:"https://i.ibb.co/whzR4XN0/Best-Indian-Punjabi-Samosa-Recipe.jpg",desc:"Fresh"},
{name:"Tea",price:15,cat:"Snacks",img:"https://i.ibb.co/Ndh5PqjW/cans.webp",desc:"Masala tea"},
{name:"Coffee",price:30,cat:"Snacks",img:"https://i.ibb.co/Ndh5PqjW/cans.webp",desc:"Hot coffee"}];
let cart=JSON.parse(localStorage.cart||"{}"),cat="All";
function save(){localStorage.cart=JSON.stringify(cart);renderCart()}
function render(){grid.innerHTML="";products.filter(p=>cat=="All"||p.cat==cat).forEach((p,i)=>grid.innerHTML+=`<div class=card><img loading="lazy" src="${p.img}"><h3>${p.name}</h3><p>₹${p.price}</p><p>${p.desc}</p><button onclick="add(${i})">Add to Cart</button></div>`)}
function filter(c){cat=c;render()}
function add(i){cart[i]=(cart[i]||0)+1;save()}
function renderCart(){let h="",t=0,c=0;for(let i in cart){let p=products[i],q=cart[i];c+=q;t+=q*p.price;h+=`${p.name} x${q} ₹${q*p.price} <button onclick="chg(${i},1)">+</button><button onclick="chg(${i},-1)">-</button><button onclick="del(${i})">X</button><br>`}items.innerHTML=h||"Empty";total.textContent="Total ₹"+t;count.textContent=c}
function chg(i,d){cart[i]+=d;if(cart[i]<=0)delete cart[i];save()}
function del(i){delete cart[i];save()}
function openCart(){panel.style.display="block";renderCart()}
function closeCart(){panel.style.display="none"}
function compileOrder(){let s="",t=0;for(let i in cart){let p=products[i];s+=`${p.name} x${cart[i]} = ₹${p.price*cart[i]}\n`;t+=p.price*cart[i]}summary.value=s+"Total: ₹"+t;localStorage.removeItem("cart")}
render();renderCart();
</script>
</body></html>""")
path="/mnt/data/index.html"
with open(path,"w",encoding="utf-8") as f:f.write(html)
print(path)

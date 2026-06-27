<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Penny Cafe - Menu</title>

<script src="https://cdn.tailwindcss.com"></script>
<link href="https://fonts.googleapis.com/css2?family=Cairo:wght@300;400;600;700&display=swap" rel="stylesheet">

<style>
*{box-sizing:border-box}
html{-webkit-tap-highlight-color:transparent}
body{
  margin:0;
  font-family:'Cairo',sans-serif;
  background:#000;
  color:#fff;
}
#app{
  width:100%;
  max-width:430px;
  min-height:100vh;
  margin:0 auto;
  padding:0 10px 20px;
  background:#000;
}
.gold-accent{color:#d4af37}
.no-scrollbar::-webkit-scrollbar{display:none}
.item-img{background:linear-gradient(45deg,#1a1a1a,#000)}
.menu-card{
  animation:fadeUp .25s ease both;
  transition:transform .2s ease,border-color .2s ease;
}
.menu-card:active{transform:scale(.98)}
@keyframes fadeUp{
  from{opacity:0;transform:translateY(8px) scale(.98)}
  to{opacity:1;transform:translateY(0) scale(1)}
}
.fullscreen-img{animation:zoomIn .25s ease both}
@keyframes zoomIn{
  from{opacity:0;transform:scale(.85)}
  to{opacity:1;transform:scale(1)}
}
</style>
</head>

<body>

<div id="app" class="flex flex-col relative">

<header class="py-5 text-center border-b border-neutral-900 bg-gradient-to-b from-neutral-950 to-black relative rounded-b-2xl">
  <button id="langBtn" onclick="toggleLanguage()" class="absolute right-3 top-3 bg-neutral-900 text-[10px] px-2.5 py-1 rounded-lg border border-neutral-700">
    EN
  </button>

  <h1 class="text-xl font-bold tracking-widest gold-accent">PENNY CAFE</h1>
</header>

<div class="sticky top-0 bg-black/90 backdrop-blur z-40 px-1 py-2.5 flex gap-1.5 overflow-x-auto no-scrollbar border-b border-neutral-900" id="category-tabs"></div>

<main class="p-1.5 flex-grow space-y-1.5" id="menu-container"></main>

</div>

<div id="imageModal" onclick="closeImageModal()" class="hidden fixed inset-0 bg-black/90 z-[999] items-center justify-center p-4">
  <img id="modalImage" class="fullscreen-img max-w-full max-h-full rounded-2xl object-contain">
</div>

<script>
let currentLanguage = localStorage.getItem("pennyLang") || "ar";

const categories = [
  { id:"all", nameAr:"الكل", nameEn:"All" },
  { id:"cold", nameAr:"مشروبات باردة", nameEn:"Cold Drinks" },
  { id:"hot", nameAr:"مشروبات ساخنة", nameEn:"Hot Drinks" },
  { id:"juices", nameAr:"عصائر طبيعية", nameEn:"Natural Juices" },
  { id:"shakes", nameAr:"ميلك شيك", nameEn:"Milkshakes" },
  { id:"pancake", nameAr:"بان كيك", nameEn:"Pancake" },
  { id:"crepe", nameAr:"كريب", nameEn:"Crepe" },
  { id:"waffle", nameAr:"وافل", nameEn:"Waffle" },
  { id:"shisha", nameAr:"شيشة", nameEn:"Shisha" },
  { id:"food", nameAr:"مأكولات", nameEn:"Food" }
];

const menuItems = [
  { nameAr:"ماء", nameEn:"Water", price:500, category:"cold", image:"imeges/1 (9).jpg" },
  { nameAr:"كولا / سبرايت / فانتا", nameEn:"Cola / Sprite / Fanta", price:1000, category:"cold", image:"imeges/1 (15).jpg" },
  { nameAr:"مشروب طاقة", nameEn:"Energy Drink", price:2000, category:"cold", image:"imeges/1 (13).jpg" },
  { nameAr:"موهيتو بلو هواي", nameEn:"Blue Hawaii Mojito", price:4000, category:"cold", image:"" },
  { nameAr:"موهيتو فراولة", nameEn:"Strawberry Mojito", price:4000, category:"cold", image:"" },
  { nameAr:"آيس كوفي", nameEn:"Iced Coffee", price:4000, category:"cold", image:"" },

  { nameAr:"شاي", nameEn:"Tea", price:500, category:"hot", image:"imeges/1 (1).jpg" },
  { nameAr:"قهوة", nameEn:"Coffee", price:2000, category:"hot", image:"" },
  { nameAr:"كابتشينو", nameEn:"Cappuccino", price:3000, category:"hot", image:"" },
  { nameAr:"هوت شوكليت", nameEn:"Hot Chocolate", price:3000, category:"hot", image:"" },
  { nameAr:"نسكافي", nameEn:"Nescafe", price:3000, category:"hot", image:"" },
  { nameAr:"حليب و نوتيلا", nameEn:"Milk with Nutella", price:3000, category:"hot", image:"" },
  { nameAr:"حليب و لوتس", nameEn:"Milk with Lotus", price:3000, category:"hot", image:"" },
  { nameAr:"حليب و فستق", nameEn:"Milk with Pistachio", price:3000, category:"hot", image:"" },
  { nameAr:"حليب و كندر", nameEn:"Milk with Kinder", price:3000, category:"hot", image:"" },

  { nameAr:"عصير برتقال", nameEn:"Orange Juice", price:4000, category:"juices", image:"" },
  { nameAr:"عصير ليمون", nameEn:"Lemon Juice", price:4000, category:"juices", image:"" },
  { nameAr:"عصير رمان", nameEn:"Pomegranate Juice", price:4000, category:"juices", image:"" },
  { nameAr:"عصير جزر", nameEn:"Carrot Juice", price:4000, category:"juices", image:"" },
  { nameAr:"عصير تفاح", nameEn:"Apple Juice", price:4000, category:"juices", image:"" },
  { nameAr:"عصير بطيخ", nameEn:"Watermelon Juice", price:4000, category:"juices", image:"" },
  { nameAr:"عصير كوكتيل", nameEn:"Cocktail Juice", price:4000, category:"juices", image:"" },

  { nameAr:"فراولة", nameEn:"Strawberry", price:5000, category:"shakes", image:"C:\Users\Penny\Pictures\1" },
  { nameAr:"نوتيلا", nameEn:"Nutella", price:5000, category:"shakes", image:"" },
  { nameAr:"اوريو", nameEn:"Oreo", price:5000, category:"shakes", image:"" },
  { nameAr:"كندر", nameEn:"Kinder", price:5000, category:"shakes", image:"" },
  { nameAr:"لوتس", nameEn:"Lotus", price:5000, category:"shakes", image:"" },

  { nameAr:"بان كيك نوتيلا", nameEn:"Nutella Pancake", price:5000, category:"pancake", image:"" },
  { nameAr:"بان كيك اوريو", nameEn:"Oreo Pancake", price:5000, category:"pancake", image:"" },
  { nameAr:"بان كيك كندر", nameEn:"Kinder Pancake", price:5000, category:"pancake", image:"" },
  { nameAr:"بان كيك لوتس", nameEn:"Lotus Pancake", price:5000, category:"pancake", image:"" },

  { nameAr:"كريب نوتيلا", nameEn:"Nutella Crepe", price:5000, category:"crepe", image:"" },
  { nameAr:"كريب اوريو", nameEn:"Oreo Crepe", price:5000, category:"crepe", image:"" },
  { nameAr:"كريب كندر", nameEn:"Kinder Crepe", price:5000, category:"crepe", image:"" },
  { nameAr:"كريب لوتس", nameEn:"Lotus Crepe", price:5000, category:"crepe", image:"" },

  { nameAr:"وافل نوتيلا", nameEn:"Nutella Waffle", price:5000, category:"waffle", image:"" },
  { nameAr:"وافل اوريو", nameEn:"Oreo Waffle", price:5000, category:"waffle", image:"" },
  { nameAr:"وافل كندر", nameEn:"Kinder Waffle", price:5000, category:"waffle", image:"" },
  { nameAr:"وافل لوتس", nameEn:"Lotus Waffle", price:5000, category:"waffle", image:"" },
  { nameAr:"وافل ستيك", nameEn:"Waffle Stick", price:5000, category:"waffle", image:"" },

  { nameAr:"تفاحتين", nameEn:"Double Apple", price:6000, category:"shisha", image:"hookah/photo_2026-06-27_04-08-48.jpg" },
  { nameAr:"انجليزي", nameEn:"English", price:6000, category:"shisha", image:"hookah/photo_2026-06-27_04-08-53.jpg" },
  { nameAr:"ليمون و نعناع", nameEn:"Lemon Mint", price:6000, category:"shisha", image:"hookah/photo_2026-06-27_04-07-55.jpg" },
  { nameAr:"علك و نعناع", nameEn:"Gum Mint", price:6000, category:"shisha", image:"hookah/photo_2026-06-27_04-08-42.jpg" },

  { nameAr:"لفة دجاج", nameEn:"Chicken Wrap", price:2000, category:"food", image:"" },
  { nameAr:"لفة لحم", nameEn:"Meat Wrap", price:3000, category:"food", image:"" },
  { nameAr:"رول صغير دجاج", nameEn:"Small Chicken Roll", price:2500, category:"food", image:"" },
  { nameAr:"رول صغير لحم", nameEn:"Small Meat Roll", price:3500, category:"food", image:"" },
  { nameAr:"دونر دجاج", nameEn:"Chicken Doner", price:4000, category:"food", image:"" },
  { nameAr:"دونر لحم", nameEn:"Meat Doner", price:6000, category:"food", image:"" },
  { nameAr:"بوكس دجاج", nameEn:"Chicken Box", price:4500, category:"food", image:"" },
  { nameAr:"بوكس لحم", nameEn:"Meat Box", price:6000, category:"food", image:"" },
  { nameAr:"رول دجاج", nameEn:"Chicken Roll", price:5000, category:"food", image:"" },
  { nameAr:"رول لحم", nameEn:"Meat Roll", price:6000, category:"food", image:"" },
  { nameAr:"رول كريسبي", nameEn:"Crispy Roll", price:6000, category:"food", image:"" },
  { nameAr:"همبرغر", nameEn:"Hamburger", price:6000, category:"food", image:"imeges/1 (1).jpg" },
  { nameAr:"كريسبي بوكس", nameEn:"Crispy Box", price:7000, category:"food", image:"" },
  { nameAr:"لحم عجين", nameEn:"Lahm Bi Ajeen", price:5000, category:"food", image:"imeges/1 (1).jpg" },
  { nameAr:"بيتزا دجاج", nameEn:"Chicken Pizza", price:9000, category:"food", image:"" },
  { nameAr:"بيتزا لحم", nameEn:"Meat Pizza", price:11000, category:"food", image:"" },
  { nameAr:"ريزو كريسبي", nameEn:"Crispy Rizo", price:6500, category:"food", image:"imeges/1 (1).jpg" },
  { nameAr:"ريزو دجاج", nameEn:"Chicken Rizo", price:6500, category:"food", image:"" },
  { nameAr:"ريزو لحم", nameEn:"Meat Rizo", price:8000, category:"food", image:"imeges/1 (1).jpg" },
  { nameAr:"فنكر", nameEn:"Finger", price:2500, category:"food", image:"" }
];

let activeCategory = "all";

function applyLanguage(){
  document.documentElement.lang = currentLanguage;
  document.documentElement.dir = currentLanguage === "ar" ? "rtl" : "ltr";
  document.getElementById("langBtn").textContent = currentLanguage === "ar" ? "EN" : "AR";
}

function toggleLanguage(){
  currentLanguage = currentLanguage === "ar" ? "en" : "ar";
  localStorage.setItem("pennyLang", currentLanguage);
  render();
}

function text(ar,en){
  return currentLanguage === "ar" ? ar : en;
}

function render(){
  applyLanguage();

  document.getElementById("category-tabs").innerHTML = categories.map(cat => `
    <button onclick="selectCategory('${cat.id}')" class="px-3 py-1 rounded-full text-[10px] font-bold transition whitespace-nowrap ${activeCategory === cat.id ? 'bg-[#d4af37] text-black' : 'bg-neutral-900 text-neutral-400'}">
      ${text(cat.nameAr, cat.nameEn)}
    </button>
  `).join("");

  document.getElementById("menu-container").innerHTML = menuItems
    .filter(item => activeCategory === "all" || item.category === activeCategory)
    .map(item => `
      <div class="menu-card bg-neutral-950 p-2 rounded-xl border border-neutral-900 hover:border-[#d4af37]/40 flex gap-2.5 items-center">
        <div onclick="${item.image ? `openImageModal('${item.image}')` : ''}" 
          class="w-10 h-10 rounded-xl item-img overflow-hidden flex items-center justify-center text-neutral-600 shrink-0 ${item.image ? 'cursor-pointer' : ''}">
          ${item.image 
            ? `<img src="${item.image}" class="w-full h-full object-cover rounded-xl">` 
            : `<span class="text-[8px] italic text-neutral-500">PHOTO</span>`}
        </div>

        <div class="flex-grow min-w-0">
          <h3 class="font-bold text-white text-[13px] leading-tight truncate">
            ${text(item.nameAr, item.nameEn || item.nameAr)}
          </h3>
          <span class="gold-accent font-semibold text-xs">
            ${Number(item.price).toLocaleString()} IQD
          </span>
        </div>
      </div>
    `).join("");
}

function selectCategory(id){
  activeCategory = id;
  render();
}

function openImageModal(src){
  document.getElementById("modalImage").src = src;
  document.getElementById("imageModal").classList.remove("hidden");
  document.getElementById("imageModal").classList.add("flex");
}

function closeImageModal(){
  document.getElementById("imageModal").classList.add("hidden");
  document.getElementById("imageModal").classList.remove("flex");
}

render();
</script>

</body>
</html>

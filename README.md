# kidsstore2
<html lang="kk">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>👑 KidsStyle Boutique — Балалар киім дүкені</title>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;800&display=swap" rel="stylesheet">
  <style>
    :root {
      --accent: #FF7EB9;
      --accent2: #FFC3A0;
      --bg1: #FFE6F7;
      --bg2: #FFF8F8;
      --card: #fff;
      font-family: 'Poppins', sans-serif;
    }
    *{margin:0;padding:0;box-sizing:border-box;}
    body{
      background:linear-gradient(135deg,var(--bg1),var(--bg2));
      color:#333;
      transition:background 0.5s ease;
    }
    .container{max-width:1100px;margin:30px auto;padding:20px;}
    header{
      display:flex;
      justify-content:space-between;
      align-items:center;
      flex-wrap:wrap;
      gap:10px;
    }
    .logo{display:flex;align-items:center;gap:12px;}
    .logo img{
      width:65px;
      height:65px;
      border-radius:50%;
      box-shadow:0 4px 10px rgba(255,160,180,0.5);
    }
    .logo span{
      font-weight:800;
      font-size:28px;
      background:linear-gradient(45deg,#FFD700,#FFB6C1);
      -webkit-background-clip:text;
      -webkit-text-fill-color:transparent;
      text-shadow:0 2px 6px rgba(255,182,193,0.5);
    }
    nav{display:flex;gap:20px;}
    nav a{text-decoration:none;color:#555;font-weight:600;transition:.3s;}
    nav a:hover{color:var(--accent);}
    button{
      cursor:pointer;
      border:none;
      border-radius:10px;
      padding:10px 18px;
      background:linear-gradient(45deg,var(--accent),var(--accent2));
      color:white;
      font-weight:600;
      box-shadow:0 4px 10px rgba(0,0,0,0.1);
      transition:.3s;
    }
    button:hover{transform:scale(1.05);}
    h2{
      margin-top:30px;
      color:#333;
      text-align:center;
      font-size:30px;
      text-shadow:0 2px 5px rgba(255,170,200,0.3);
    }
    .products{
      display:grid;
      grid-template-columns:repeat(auto-fit,minmax(250px,1fr));
      gap:25px;
      margin-top:30px;
    }
    .card{
      background:var(--card);
      border-radius:16px;
      box-shadow:0 4px 15px rgba(255,160,190,0.3);
      overflow:hidden;
      transition:0.3s;
    }
    .card:hover{transform:translateY(-5px) scale(1.02);}
    .card img{width:100%;height:230px;object-fit:cover;}
    .info{padding:15px;}
    .info h3{font-size:18px;margin-bottom:6px;color:#333;}
    .info p{font-size:14px;color:#666;margin-bottom:8px;}
    .price{font-weight:700;color:var(--accent);margin-bottom:10px;}
    footer{margin-top:40px;text-align:center;color:#888;font-size:14px;}
    #cartModal{
      display:none;
      position:fixed;
      top:0;left:0;width:100%;height:100%;
      background:rgba(0,0,0,0.6);
      justify-content:center;
      align-items:center;
      z-index:10;
    }
    .cart-box{
      background:#fff;
      padding:25px;
      border-radius:15px;
      width:90%;
      max-width:420px;
      box-shadow:0 6px 20px rgba(0,0,0,0.2);
    }
    .cart-item{display:flex;justify-content:space-between;margin-bottom:10px;}
    .cart-total{font-weight:bold;margin-top:10px;}
    .actions{text-align:right;margin-top:10px;}
    .sale{
      background:linear-gradient(45deg,#FFD6E0,#FFF5E4);
      padding:25px;
      border-radius:15px;
      margin-top:40px;
      text-align:center;
      box-shadow:0 4px 12px rgba(255,180,190,0.4);
    }
    .sale h3{color:#E63946;margin-bottom:10px;font-size:22px;}
    .sale p{color:#555;}
  </style>
</head>
<body>
  <div class="container">
    <header>
      <div class="logo">
        <img src="https://cdn-icons-png.flaticon.com/512/7321/7321025.png" alt="logo">
        <span>KidsStyle Boutique</span>
      </div>
      <nav>
        <a href="#">Басты бет</a>
        <a href="#sales">Акциялар</a>
      </nav>
      <button id="openCart">🛒 Себет (<span id="cartCount">0</span>)</button>
    </header>

    <h2>🧸 Біздің сүйкімді өнімдер</h2>
    <div class="products" id="productList"></div>

    <section id="sales" class="sale">
      <h3>🎁 Акциялар мен жеңілдіктер</h3>
      <p>🌟 2 өнім алсаңыз — 3-ші 50% жеңілдік!</p>
      <p>🚚 20 000 ₸ жоғары тапсырыс — тегін жеткізу!</p>
    </section>

    <footer>© 2025 KidsStyle Boutique — Махаббатпен жасалған 💕</footer>
  </div>

  <!-- Себет терезесі -->
  <div id="cartModal">
    <div class="cart-box">
      <h3>🛍 Сіздің себетіңіз</h3>
      <div id="cartItems"></div>
      <div class="cart-total">Жалпы: <span id="totalPrice">0</span> ₸</div>
      <div class="actions">
        <button id="orderBtn">Тапсырыс беру</button>
        <button id="closeCart">Жабу</button>
      </div>
    </div>
  </div>

  <script>
    const products = [
      {name:'Жайлы футболка',desc:'100% мақтадан тігілген',price:4200,img:'https://i.pinimg.com/1200x/5c/43/85/5c4385f27407362d0c7243a4d2b515db.jpg'},
      {name:'Спортивкалар',desc:'Жылы және жеңіл',price:8900,img:'https://i.pinimg.com/736x/16/5b/a7/165ba78da26bd81487389c211236b45d.jpg'},
      {name:'Жеңіл күртешелер',desc:'Ыңғайлы және сәнді',price:3500,img:'https://i.pinimg.com/1200x/c7/dc/81/c7dc811ba6cc7601786cc039e45cd7d7.jpg'},
      {name:'Қыздарға көйлек',desc:'Жеңіл және әдемі',price:5600,img:'https://i.pinimg.com/736x/ba/58/2b/ba582b08b2e66db9886f9f1ef3f2650a.jpg'},
      {name:'Комбинезон',desc:'Күнделікті киюге арналған',price:7200,img:'https://i.pinimg.com/736x/b6/49/f0/b649f0a832f16368836c20dcc8853b46.jpg'},
      {name:'Сәнді рубашкалар',desc:'Қыста киюге тамаша',price:9800,img:'https://i.pinimg.com/1200x/38/6d/44/386d44a43f0c8d75f3ffdcbf7d908be2.jpg'}
    ];

    const productList=document.getElementById('productList');
    const cartModal=document.getElementById('cartModal');
    const cartItems=document.getElementById('cartItems');
    const cartCount=document.getElementById('cartCount');
    const totalPrice=document.getElementById('totalPrice');
    let cart=[];

    products.forEach((p,i)=>{
      const card=document.createElement('div');
      card.className='card';
      card.innerHTML=`
        <img src="${p.img}" alt="">
        <div class="info">
          <h3>${p.name}</h3>
          <p>${p.desc}</p>
          <div class="price">${p.price} ₸</div>
          <button onclick="addToCart(${i})">🛍 Себетке қосу</button>
        </div>`;
      productList.appendChild(card);
    });

    function addToCart(i){
      cart.push(products[i]);
      cartCount.textContent=cart.length;
      alert(products[i].name + " себетке қосылды 💖");
    }

    document.getElementById('openCart').onclick=()=>{
      cartModal.style.display='flex';
      renderCart();
    };
    document.getElementById('closeCart').onclick=()=>{
      cartModal.style.display='none';
    };

    function renderCart(){
      cartItems.innerHTML='';
      let total=0;
      cart.forEach((item,index)=>{
        total+=item.price;
        const row=document.createElement('div');
        row.className='cart-item';
        row.innerHTML=`
          <span>${item.name}</span>
          <span>${item.price} ₸ <button onclick="removeItem(${index})">x</button></span>`;
        cartItems.appendChild(row);
      });
      totalPrice.textContent=total;
    }

    function removeItem(i){
      cart.splice(i,1);
      cartCount.textContent=cart.length;
      renderCart();
    }

    document.getElementById('orderBtn').onclick=()=>{
      if(cart.length===0){alert("Себет бос 😅");return;}
      let total=cart.reduce((a,b)=>a+b.price,0);
      alert("💖 Тапсырысыңыз қабылданды! Жалпы сома: "+total+" ₸. Рахмет!");
      cart=[];
      cartCount.textContent=0;
      renderCart();
    };
  </script>
</body>
</html>

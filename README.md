<!DOCTYPE html>
<html lang="zh-Hant">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Luck CCD SHOP- Official</title>
    <script type="text/javascript" src="https://cdn.jsdelivr.net/npm/@emailjs/browser@4/dist/email.min.js"></script>
    <style>
        :root { 
            --bg: #fce4ec;       /* 淺粉紅背景 */
            --main: #1a237e;     /* 深藍色主題文字 */
            --white: #ffffff; 
            --grey: #f5f5f5;
            --dark-btn: #0b3c4d; /* 結帳深色按鈕 */
        }
        body { background: var(--bg); color: var(--main); font-family: "Times New Roman", serif; margin: 0; padding-bottom: 120px; }
        
        .announcement { background: var(--main); color: white; text-align: center; padding: 8px; font-size: 12px; font-family: sans-serif; }
        
        /* Navbar 頂部 */
        .navbar { display: flex; justify-content: space-between; align-items: center; padding: 15px 20px; background: transparent; }
        .nav-left, .nav-right { display: flex; align-items: center; gap: 15px; }
        .icon-btn { background: none; border: none; color: var(--main); font-size: 1.5rem; cursor: pointer; font-weight: bold; }
        
        header { text-align: center; padding: 10px 10px 20px 10px; }
        header h1 { font-size: 2.6rem; margin: 0; letter-spacing: 1px; color: #ec407a; word-break: break-word; }
        header p { font-size: 1.4rem; margin: 5px 0 0 0; font-family: "Times New Roman", serif; color: var(--main); font-weight: bold; }

        /* 📸 容器包裝器 */
        .hero-container {
            position: relative;
            width: 92%;
            max-width: 800px;
            margin: 0 auto 20px auto;
            border-radius: 8px;
            overflow: hidden;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }
        .hero-image {
            width: 100%;
            display: block;
            aspect-ratio: 4 / 3;
            object-fit: cover;
            background-color: #ddd; 
        }
        .hero-overlay {
            position: absolute;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0, 0, 0, 0.1);
            display: flex;
            justify-content: center;
            align-items: center;
        }
        .shop-now-btn {
            background-color: var(--white);
            color: var(--main);
            border: 2px solid var(--main);
            padding: 12px 35px;
            font-size: 1.2rem;
            font-weight: bold;
            border-radius: 30px;
            cursor: pointer;
            box-shadow: 0 4px 10px rgba(0,0,0,0.2);
            transition: all 0.2s ease;
            font-family: sans-serif;
        }
        .shop-now-btn:active {
            transform: scale(0.95);
            background-color: var(--main);
            color: var(--white);
        }

        /* 📥 粉紅色側欄選單 */
        .sidebar { 
            position: fixed; top: 0; left: -280px; width: 280px; height: 100%; 
            background: var(--bg); 
            z-index: 2000; transition: 0.3s; box-shadow: 2px 0 10px rgba(0,0,0,0.1); padding: 20px; box-sizing: border-box; font-family: sans-serif; 
            overflow-y: auto; 
            -webkit-overflow-scrolling: touch; 
        }
        .sidebar.active { left: 0; }
        .sidebar-close { text-align: right; font-size: 1.5rem; cursor: pointer; margin-bottom: 20px; color: var(--main); }
        .menu-item { display: block; padding: 12px 0; color: var(--main); text-decoration: none; font-weight: bold; border-bottom: 1px solid rgba(26, 35, 126, 0.2); cursor: pointer; }
        .sub-menu { padding-left: 15px; display: none; }
        .sub-menu.active { display: block; }
        .sub-menu-item { display: block; padding: 8px 0; color: #333; text-decoration: none; font-size: 14px; cursor: pointer; }

        /* 搜尋欄 */
        .search-container { text-align: center; padding: 10px; display: none; font-family: sans-serif; }
        .search-container input { width: 80%; max-width: 400px; padding: 10px; border: 1.5px solid var(--main); border-radius: 20px; outline: none; }

        /* 商品網格 */
        .grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; padding: 12px; max-width: 800px; margin: 0 auto; }
        .card { background: var(--white); padding-bottom: 15px; text-align: center; border-radius: 4px; box-shadow: 0 2px 5px rgba(0,0,0,0.05); overflow: hidden; }
        .card img { width: 100%; aspect-ratio: 1; object-fit: cover; background: var(--grey); }
        .brand-tag { font-size: 10px; text-transform: uppercase; color: #888; margin-top: 8px; font-family: sans-serif; }
        .name { font-weight: bold; margin: 5px; height: 35px; font-size: 0.9rem; overflow: hidden; color: var(--main); }
        .price { margin-bottom: 12px; font-size: 1rem; font-weight: bold; }

        /* 按鈕樣式 */
        .btn { border: 1.5px solid var(--main); background: none; color: var(--main); padding: 6px 18px; border-radius: 25px; font-weight: bold; cursor: pointer; font-size: 0.85rem; }
        .btn:active { background: var(--main); color: white; }

        /* 共通 Modal */
        .modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.7); z-index: 3000; }
        .modal-content { background: white; margin: 8% auto; padding: 20px; width: 85%; max-width: 420px; border-radius: 8px; color: #333; max-height: 80vh; overflow-y: auto; font-family: sans-serif; position: relative; }
        
        /* 結帳頁面的 QR 樣式 */
        .qr-code { width: 180px; height: 180px; background: #eee; margin: 15px auto; display: none; border: 1px solid #ddd; border-radius: 4px; object-fit: contain; }

        /* 🛒 獨立購物車頁面與結帳頁面樣式 */
        .page-section-container {
            max-width: 500px;
            margin: 0 auto;
            padding: 20px;
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
        }
        .page-cart-header {
            display: flex;
            justify-content: space-between;
            align-items: baseline;
            margin-bottom: 30px;
        }
        .page-cart-header h2 {
            font-size: 2.2rem;
            margin: 0;
            color: var(--main);
            font-family: "Times New Roman", serif;
        }
        .continue-shopping-link {
            color: var(--main);
            text-decoration: underline;
            font-size: 14px;
            cursor: pointer;
        }
        .table-labels {
            display: flex;
            justify-content: space-between;
            font-size: 11px;
            color: #888;
            letter-spacing: 1px;
            text-transform: uppercase;
            border-bottom: 1px solid rgba(0,0,0,0.1);
            padding-bottom: 8px;
            margin-bottom: 20px;
        }
        
        /* 購物車品項 */
        .custom-cart-item {
            display: flex;
            gap: 15px;
            padding: 20px 0;
            border-bottom: 1px solid rgba(0,0,0,0.05);
            align-items: flex-start;
        }
        .custom-cart-item img {
            width: 90px;
            height: 90px;
            object-fit: cover;
            border-radius: 4px;
            background: #eee;
        }
        .custom-cart-details {
            flex: 1;
        }
        .custom-cart-name {
            font-size: 15px;
            font-weight: bold;
            color: var(--main);
            margin-bottom: 5px;
            line-height: 1.3;
        }
        .custom-cart-price {
            font-size: 14px;
            color: #666;
            margin-bottom: 15px;
        }
        .custom-cart-actions {
            display: flex;
            align-items: center;
            gap: 0;
        }
        .qty-wrapper {
            display: flex;
            align-items: center;
            border: 1px solid rgba(0,0,0,0.1);
            border-radius: 4px;
            background: transparent;
            overflow: hidden;
        }
        .qty-btn {
            background: none;
            border: none;
            padding: 8px 14px;
            cursor: pointer;
            font-size: 16px;
            color: #555;
        }
        .qty-num {
            padding: 0 10px;
            font-size: 14px;
            min-width: 20px;
            text-align: center;
        }
        .delete-icon-btn {
            background: none;
            border: none;
            margin-left: 20px;
            cursor: pointer;
            color: #666;
            font-size: 1.1rem;
        }

        .total-display-row {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin: 30px 0;
            font-size: 18px;
        }
        .total-display-row .label {
            font-weight: bold;
            color: var(--main);
        }
        .total-display-row .amount {
            font-size: 18px;
            font-weight: normal;
            color: #444;
        }

        /* 大黑藍色按鈕 */
        .checkout-main-btn {
            width: 100%;
            background-color: #0b3c4d;
            color: white;
            border: none;
            padding: 16px;
            font-size: 16px;
            font-weight: bold;
            border-radius: 30px;
            cursor: pointer;
            text-align: center;
            letter-spacing: 1px;
            margin-top: 10px;
        }
        .checkout-main-btn:active {
            background-color: #06232e;
        }

        /* 結帳頁面專用樣式 */
        .checkout-title-area {
            text-align: center;
            margin-bottom: 30px;
        }
        .checkout-shop-title {
            font-size: 1.8rem;
            margin: 0 0 5px 0;
            color: var(--main);
            font-family: "Times New Roman", serif;
        }
        .checkout-page-title {
            font-size: 1.3rem;
            margin: 0;
            color: #666;
            font-weight: normal;
        }
        .checkout-summary-box {
            background: white;
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 25px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.05);
        }
        .checkout-summary-item {
            display: flex;
            justify-content: space-between;
            font-size: 14px;
            padding: 8px 0;
            border-bottom: 1px dashed #eee;
        }
        .checkout-summary-item:last-child {
            border-bottom: none;
        }
        
        /* 選擇付款方式下拉選單 */
        .payment-select-dropdown {
            width: 100%;
            padding: 12px;
            font-size: 15px;
            border: 1px solid rgba(26, 35, 126, 0.2);
            border-radius: 4px;
            background-color: #fff;
            color: #333;
            outline: none;
            margin-top: 5px;
        }

        /* 表單與上傳欄位樣式 */
        .contact-form input, .contact-form textarea, .sf-form input, .custom-textarea-input { 
            width: 100%; padding: 12px; margin: 8px 0; border: 1px solid rgba(26, 35, 126, 0.2); 
            border-radius: 4px; box-sizing: border-box; background-color: #fffbfe; font-size: 15px;
        }
        .contact-form textarea { height: 100px; resize: none; }
        
        /* 增加紅色星號標記必填 */
        .required-label::after {
            content: " *";
            color: red;
            font-weight: bold;
        }

        /* Shopify 經典登入彈窗 */
        .login-modal-content {
            background: white;
            margin: 12% auto;
            padding: 35px 30px;
            width: 88%;
            max-width: 400px;
            border-radius: 16px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
            color: #333;
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
            box-sizing: border-box;
        }
        .shopify-login-box { display: block; width: 100%; text-align: left; }
        .shopify-title { font-size: 32px; font-weight: 700; color: #1a1a1a; margin: 0 0 10px 0; letter-spacing: -0.5px; }
        .shopify-subtitle { font-size: 15px; color: #616161; margin-bottom: 28px; line-height: 1.4; }
        .shopify-label { font-size: 13px; font-weight: 600; color: #1a1a1a; display: block; margin-bottom: 8px; }
        .shopify-input { 
            width: 100%; padding: 14px 16px; margin: 0 0 20px 0; border: 1px solid #b5b5b5; 
            border-radius: 8px; box-sizing: border-box; font-size: 16px; background: #fff; transition: border 0.2s;
        }
        .shopify-input:focus { border: 2px solid #000; outline: none; }
        .shopify-btn { 
            background: #1a1a1a; color: white; border: none; width: 100%; padding: 15px; 
            border-radius: 8px; font-size: 16px; font-weight: 600; cursor: pointer; margin-top: 5px; transition: background 0.2s;
            display: block; text-align: center;
        }
        .shopify-btn:hover { background: #333; }
        .shopify-btn:disabled { background: #aaa; cursor: not-allowed; }
        
        .code-container { display: flex; justify-content: space-between; gap: 8px; margin: 25px 0; }
        .code-input { width: 45px; height: 50px; text-align: center; font-size: 22px; font-weight: bold; border: 1px solid #b5b5b5; border-radius: 8px; background: #fff; }
        .code-input:focus { border: 2px solid #000; outline: none; }

        .footer-bar { position: fixed; bottom: 0; width: 100%; background: var(--main); color: white; padding: 15px; text-align: center; font-family: sans-serif; font-weight: bold; font-size: 1.1rem; cursor: pointer; z-index: 90; box-sizing: border-box; }

        /* 🔒 管理員專用區塊 */
        .admin-only { display: none; }
        .admin-box { background: #eee; padding: 15px; margin: 20px; border: 2px dashed var(--main); border-radius: 6px; font-family: sans-serif; }
        .admin-box input, .admin-box select { width: 100%; padding: 8px; margin: 5px 0; border: 1px solid #aaa; }
        .admin-action-area { text-align: center; margin: 10px auto; max-width: 800px; }
        .upload-hint { font-size: 12px; color: #666; font-family: sans-serif; margin-top: 4px; }
    </style>
</head>
<body>

<div class="announcement">滿$1000即順豐包郵 Free Shipping Over $1000 📦</div>

<div class="navbar">
    <div class="nav-left">
        <button class="icon-btn" onclick="toggleSidebar()">☰</button>
    </div>
    <div class="nav-right">
        <button class="icon-btn" onclick="toggleSearch()">🔍</button>
        <button class="icon-btn" onclick="showSection('cart')">🛒 (<span id="icon-cart-count">0</span>)</button>
    </div>
</div>

<div id="main-header-area">
    <header>
        <h1 id="main-title">Welcome to Lucky.ccd_shop</h1>
        <p id="page-title" onclick="showSection('home')" style="cursor:pointer;">Home ﹀</p>
    </header>
</div>

<div class="search-container" id="search-box">
    <input type="text" id="search-input" placeholder="輸入你想搵嘅產品型號..." oninput="searchProducts()">
</div>

<div class="sidebar" id="sidebar-menu">
    <div class="sidebar-close" onclick="toggleSidebar()">✕</div>
    
    <div class="menu-item" onclick="showSection('home'); toggleSidebar();">Home</div>
    <div class="menu-item" onclick="showSection('products'); filterProducts('all', 'Products'); toggleSidebar();">Shop all</div>
    
    <div class="menu-item" onclick="toggleSubMenu('brands-sub')">Brands ▾</div>
    <div class="sub-menu" id="brands-sub">
        <div class="sub-menu-item" onclick="showSection('products'); filterProducts('brand', 'Canon', 'Canon'); toggleSidebar();">Canon</div>
        <div class="sub-menu-item" onclick="showSection('products'); filterProducts('brand', 'Fujifilm', 'Fujifilm'); toggleSidebar();">Fujifilm</div>
        <div class="sub-menu-item" onclick="showSection('products'); filterProducts('brand', 'Casio', 'Casio'); toggleSidebar();">Casio</div>
        <div class="sub-menu-item" onclick="showSection('products'); filterProducts('brand', 'Sony', 'Sony'); toggleSidebar();">Sony</div>
        <div class="sub-menu-item" onclick="showSection('products'); filterProducts('brand', 'Lumix', 'Lumix'); toggleSidebar();">Lumix</div>
        <div class="sub-menu-item" onclick="showSection('products'); filterProducts('brand', 'Other', 'Others Brands'); toggleSidebar();">Others brands</div>
    </div>
    
    <div class="menu-item" onclick="showSection('products'); filterProducts('price', 500, 800, 'Cameras between $500-$800'); toggleSidebar();">Cameras between $500-$800</div>
    <div class="menu-item" onclick="showSection('products'); filterProducts('price', 800, 1000, 'Cameras between $800-$1000'); toggleSidebar();">Cameras between $800-$1000</div>
    <div class="menu-item" onclick="showSection('products'); filterProducts('price', 1000, 99999, 'Cameras above $1000'); toggleSidebar();">Cameras above $1000</div>
    
    <div class="menu-item" onclick="showSection('products'); filterProducts('brand', 'Film cameras', 'Film cameras'); toggleSidebar();">Film cameras</div>
    <div class="menu-item" onclick="showSection('products'); filterProducts('brand', 'DV camcorders', 'DV camcorders'); toggleSidebar();">DV camcorders</div>
    
    <div class="menu-item" onclick="toggleSubMenu('acc-sub')">Accessories ▾</div>
    <div class="sub-menu" id="acc-sub">
        <div class="sub-menu-item" onclick="showSection('products'); filterProducts('brand', 'Memory cards', 'Memory cards'); toggleSidebar();">Memory cards</div>
        <div class="sub-menu-item" onclick="showSection('products'); filterProducts('brand', 'Card readers', 'Card readers'); toggleSidebar();">Card readers</div>
        <div class="sub-menu-item" onclick="showSection('products'); filterProducts('brand', 'Camera bags', 'Camera bags'); toggleSidebar();">Camera bags</div>
        <div class="sub-menu-item" onclick="showSection('products'); filterProducts('brand', 'Camera chains', 'Camera chains'); toggleSidebar();">Camera chains</div>
    </div>

    <div class="menu-item" onclick="showSection('policy', 'Shopping Notice'); toggleSidebar();">Shopping Notice</div>
    
    <div class="menu-item" onclick="showSection('contact', 'Contact Us'); toggleSidebar();">Content us</div>
</div>

<!-- 首頁區塊 -->
<div id="home-section">
    <div class="admin-action-area admin-only">
        <button class="btn" style="font-size:11px; padding:4px 12px; margin-bottom:8px;" onclick="changeHomePhoto()">📸 更改首頁相片</button>
        <input type="file" id="home-file-input" accept="image/*" style="display:none;" onchange="handleHomePhotoUpload(this)">
    </div>
    <div class="hero-container">
        <img src="" id="home-hero-img" class="hero-image" alt="尚未上傳首頁相片">
        <div class="hero-overlay">
            <button class="shop-now-btn" onclick="showSection('products'); filterProducts('all', 'Products');">Shop now</button>
        </div>
    </div>
</div>

<!-- 購物須知區塊 -->
<div id="policy-section" style="display: none;">
    <div class="admin-action-area admin-only">
        <button class="btn" style="font-size:11px; padding:4px 12px; margin-bottom:8px;" onclick="changeNoticePhoto()">📸 更改購物須知圖片</button>
        <input type="file" id="notice-file-input" accept="image/*" style="display:none;" onchange="handleNoticePhotoUpload(this)">
    </div>
    <div class="hero-container">
        <img src="https://via.placeholder.com/600x800?text=Shopping+Notice+Image" id="notice-hero-img" class="hero-image" style="aspect-ratio: auto; max-height:80vh; object-fit:contain;" alt="購物須知">
    </div>
</div>

<!-- 產品列表區塊 -->
<div id="products-section" style="display: none;">
    <div class="grid" id="product-grid">
        <div class="card" data-brand="Canon" data-price="1580" data-img="https://images.unsplash.com/photo-1516035069371-29a1b244cc32?w=400">
            <div class="brand-tag">Canon</div>
            <img src="https://images.unsplash.com/photo-1516035069371-29a1b244cc32?w=400">
            <div class="name">Canon IXUS 185 (IXY 200)</div>
            <div class="price">HK$1,580.00</div>
            <button class="btn" onclick="addToCart('Canon IXUS 185 (IXY 200)', 1580, 'https://images.unsplash.com/photo-1516035069371-29a1b244cc32?w=400')">Add to cart</button>
        </div>
        <div class="card" data-brand="Film cameras" data-price="450" data-img="https://via.placeholder.com/400?text=Film+Camera">
            <div class="brand-tag">Film cameras</div>
            <img src="https://via.placeholder.com/400?text=Film+Camera">
            <div class="name">Retro 35mm Film Camera</div>
            <div class="price">HK$450.00</div>
            <button class="btn" onclick="addToCart('Retro 35mm Film Camera', 450, 'https://via.placeholder.com/400?text=Film+Camera')">Add to cart</button>
        </div>
        <div class="card" data-brand="DV camcorders" data-price="1200" data-img="https://via.placeholder.com/400?text=DV+Camcorder">
            <div class="brand-tag">DV camcorders</div>
            <img src="https://via.placeholder.com/400?text=DV+Camcorder">
            <div class="name">Vintage Y2K DV Camcorder</div>
            <div class="price">HK$1,200.00</div>
            <button class="btn" onclick="addToCart('Vintage Y2K DV Camcorder', 1200, 'https://via.placeholder.com/400?text=DV+Camcorder')">Add to cart</button>
        </div>
    </div>
</div>

<!-- 聯絡我們區塊 -->
<div id="contact-section" style="display: none; max-width: 500px; margin: 0 auto; padding: 20px; font-family: sans-serif;">
    <h2 style="font-style: italic; font-family: serif; color: var(--main); margin-bottom: 25px;">Contact Us</h2>
    <div class="contact-form">
        <input type="text" id="ct-name" placeholder="Name">
        <input type="email" id="ct-email" placeholder="Email *">
        <input type="tel" id="ct-phone" placeholder="Phone number">
        <textarea id="ct-comment" placeholder="Comment"></textarea>
        <button class="send-btn" onclick="sendContactForm()">Send</button>
    </div>
</div>

<!-- 🛒 獨立購物車頁面區塊 -->
<div id="cart-page-section" style="display: none;">
    <div class="page-section-container">
        <div class="page-cart-header">
            <h2>Your cart</h2>
            <div class="continue-shopping-link" onclick="showSection('products'); filterProducts('all', 'Products');">Continue shopping</div>
        </div>
        
        <div id="page-login-status-bar" style="font-size:12px; background:#e8eaf6; padding:8px; margin-bottom:20px; border-radius:4px;">
            💡 想要儲存購物車？ <button class="btn" style="padding:2px 8px; font-size:11px;" onclick="openLoginModal()">Login</button>
        </div>

        <div class="table-labels">
            <span>Product</span>
            <span>Total</span>
        </div>

        <div id="page-cart-items-container"></div>

        <div id="page-empty-cart-view" style="text-align:center; display:none; padding:4px 0;">
            <p style="color:#999; font-style: italic;">Your cart is empty</p>
        </div>

        <div id="page-cart-summary-area">
            <div class="total-display-row">
                <span class="label">Estimated total</span>
                <span class="amount" id="page-cart-estimated-total">HK$0.00</span>
            </div>
            <button class="checkout-main-btn" onclick="showSection('checkout')">Check out</button>
        </div>
    </div>
</div>

<!-- 💳 獨立結帳頁面區塊 -->
<div id="checkout-page-section" style="display: none;">
    <div class="page-section-container">
        <div class="checkout-title-area">
            <h2 class="checkout-shop-title">Lucky CCD Shop</h2>
            <h3 class="checkout-page-title">Check out</h3>
        </div>

        <p style="font-weight: bold; margin-bottom: 10px; font-size: 15px; color: var(--main);">加購的產品及總價：</p>
        <div class="checkout-summary-box" id="checkout-products-summary">
            <!-- 由 JS 動態渲染產品清單與總價 -->
        </div>

        <div id="checkout-payment-form-area">
            <!-- 付款方式下拉選單 -->
            <p style="margin-bottom: 5px;"><b>1. Payment methods:</b></p>
            <select id="payment-method-select" class="payment-select-dropdown" onchange="switchPaymentQR()">
                <option value="alipay">1. Alipay HK</option>
                <option value="wechat">2. WeChat Pay</option>
                <option value="fps">3. FPS</option>
            </select>

            <!-- 三個收款 QR Code (內嵌動態切換顯示) -->
            <img src="https://via.placeholder.com/200?text=Alipay+QR" id="qr-img-alipay" class="qr-code">
            <img src="https://via.placeholder.com/200?text=WeChat+QR" id="qr-img-wechat" class="qr-code">
            <img src="https://via.placeholder.com/200?text=FPS+QR" id="qr-img-fps" class="qr-code">

            <!-- 專屬提示文字 -->
            <p style="font-size:13px; color:red; font-weight: bold; margin: 15px 0 5px 0; line-height: 1.4;">
                *請付款後請截圖，並在過數時留言相機品牌及型號
            </p>
            <p style="font-size:13px; color:var(--main); font-weight: bold; margin: 0 0 15px 0; line-height: 1.4;">
                **發貨前會通過ig傳送check機片
            </p>

            <hr style="border: 0; border-top: 1px solid rgba(0,0,0,0.1); margin: 20px 0;">
            
            <!-- ✍️ 新增：過數對數與順豐填寫區塊 -->
            <p><b class="required-label">2. 填寫核對與收件資料：</b></p>
            <div class="sf-form">
                <input type="text" id="checkout-ig-camera" placeholder="IG Name + 相機型號 *">
                <input type="text" id="sf-name" placeholder="收件人姓名 *">
                <input type="tel" id="sf-phone" placeholder="聯絡電話 *">
                <input type="text" id="sf-address" placeholder="順豐收件地址/網點編號 *">
            </div>
            
            <button class="checkout-main-btn" style="border-radius: 4px; margin-top: 20px;" onclick="submitOrder()">確定付款並傳送資料</button>
            <button class="btn" style="width:100%; margin-top:10px; border:none; color:#777;" onclick="showSection('cart')">返回購物車修改</button>
        </div>
    </div>
</div>

<!-- 底部固定提示列 -->
<div id="footer-bar" class="footer-bar" style="display:none;" onclick="showSection('cart')">
    查看購物車及結帳 (共 HK$<span id="cart-total-price">0</span>) →
</div>

<!-- 登入彈窗 -->
<div id="login-modal" class="modal">
    <div class="login-modal-content">
        <div id="login-step-email" class="shopify-login-box">
            <h2 class="shopify-title">Sign in</h2>
            <div class="shopify-subtitle">Sign in or create an account</div>
            <label class="shopify-label">Email</label>
            <input type="email" id="login-email" class="shopify-input" placeholder="Enter your email address">
            <button class="shopify-btn" id="email-submit-btn" onclick="goToVerificationStep()">Continue</button>
        </div>
        <div id="login-step-code" class="shopify-login-box" style="display: none;">
            <h2 class="shopify-title">Enter verification code</h2>
            <div class="shopify-subtitle">Sent to <b id="display-target-email" style="color:#000;">email@example.com</b></div>
            <div class="code-container">
                <input type="text" class="code-input" maxlength="1" oninput="moveCodeFocus(this, 0)" onkeydown="handleCodeBackspace(event, 0)">
                <input type="text" class="code-input" maxlength="1" oninput="moveCodeFocus(this, 1)" onkeydown="handleCodeBackspace(event, 1)">
                <input type="text" class="code-input" maxlength="1" oninput="moveCodeFocus(this, 2)" onkeydown="handleCodeBackspace(event, 2)">
                <input type="text" class="code-input" maxlength="1" oninput="moveCodeFocus(this, 3)" onkeydown="handleCodeBackspace(event, 3)">
                <input type="text" class="code-input" maxlength="1" oninput="moveCodeFocus(this, 4)" onkeydown="handleCodeBackspace(event, 4)">
                <input type="text" class="code-input" maxlength="1" oninput="moveCodeFocus(this, 5)" onkeydown="handleCodeBackspace(event, 5)">
            </div>
            <button class="shopify-btn" onclick="verifyAndLogin()">Submit</button>
            <div style="text-align: center; margin-top: 25px;">
                <a href="#" style="font-size: 14px; color: #616161; text-decoration: underline;" onclick="backToEmailStep()">Back to email</a>
            </div>
        </div>
        <button class="btn" style="width:100%; margin-top:25px; border:none; color:#999; font-size:14px;" onclick="closeLoginModal()">Cancel</button>
    </div>
</div>

<!-- 後台管理 -->
<div class="admin-box admin-only">
    <h4 style="margin:0; color:var(--main);">🛠 手機後台：直接新增商品工具 (已登入店主帳戶)</h4>
    <select id="adm-brand">
        <option value="Canon">Canon</option>
        <option value="Nikon">Nikon</option>
        <option value="Fujifilm">Fujifilm</option>
        <option value="Casio">Casio</option>
        <option value="Sony">Sony</option>
        <option value="Lumix">Lumix</option>
        <option value="Other">Other brands</option>
        <option value="Film cameras">Film cameras</option>
        <option value="DV camcorders">DV camcorders</option>
    </select>
    <div style="margin: 8px 0;">
        <label style="font-size:12px; font-weight:bold; display:block; margin-bottom:4px;">商品相片：</label>
        <input type="file" id="adm-file-input" accept="image/*" onchange="handleProductPhotoUpload(this)">
        <div id="upload-status" class="upload-hint">未選擇檔案</div>
    </div>
    <input type="hidden" id="adm-img" value=""> 
    <input type="text" id="adm-name" placeholder="產品全名">
    <input type="number" id="adm-price" placeholder="價格">
    <button class="btn" style="background:var(--main); color:white; width:100%; margin-top:5px;" onclick="adminAddProduct()">網頁預覽上架</button>
    <textarea id="adm-output" style="width:100%; height:40px; font-size:10px; margin-top:5px;" readonly onclick="this.select()"></textarea>

    <hr style="border:0; border-top:1px solid #ccc; margin:15px 0;">
    <h4 style="margin:0 0 10px 0; color:var(--main);">💳 更換結帳收款二維碼 (QR Code)</h4>
    <div style="font-size:12px; margin-bottom:10px;">
        Alipay QR: <input type="file" accept="image/*" onchange="uploadPaymentQR(this, 'alipay')"><br>
        WeChat QR: <input type="file" accept="image/*" onchange="uploadPaymentQR(this, 'wechat')"><br>
        FPS QR: <input type="file" accept="image/*" onchange="uploadPaymentQR(this, 'fps')">
    </div>
</div>

<script>
    const EMAILJS_PUBLIC_KEY = "TKkLG4Rp6GHOM0o2c";       
    const EMAILJS_SERVICE_ID = "service_1t2itm5";       
    const EMAILJS_TEMPLATE_ID = "template_n7eertz"; 
    const ADMIN_EMAIL = "luckyccdshop@gmail.com"; 
    const IMGUR_CLIENT_ID = "5440db004d61554"; 

    // 🌟 已經為您更新成正確的 IG 帳號名稱！
    const instagramUsername = "Lucky_ccd.shop"; 

    emailjs.init({ publicKey: EMAILJS_PUBLIC_KEY });

    let cart = {}; 
    let currentEmail = "";
    let realSecurityCode = ""; 

    window.onload = function() {
        let savedHomeImg = localStorage.getItem("lucky_home_photo");
        if (savedHomeImg) document.getElementById('home-hero-img').src = savedHomeImg;
        
        let savedNoticeImg = localStorage.getItem("lucky_notice_photo");
        if (savedNoticeImg) document.getElementById('notice-hero-img').src = savedNoticeImg;
        
        loadPaymentQR();
        checkAdminVisibility();
        renderCart();
    };

    function checkAdminVisibility() {
        let adminElements = document.querySelectorAll('.admin-only');
        if (currentEmail.toLowerCase().trim() === ADMIN_EMAIL.toLowerCase().trim()) {
            adminElements.forEach(el => el.style.display = 'block');
        } else {
            adminElements.forEach(el => el.style.display = 'none');
        }
    }

    function loadPaymentQR() {
        let alipay = localStorage.getItem("lucky_qr_alipay") || "https://via.placeholder.com/200?text=Alipay+QR";
        let wechat = localStorage.getItem("lucky_qr_wechat") || "https://via.placeholder.com/200?text=WeChat+QR";
        let fps = localStorage.getItem("lucky_qr_fps") || "https://via.placeholder.com/200?text=FPS+QR";
        
        document.getElementById('qr-img-alipay').src = alipay;
        document.getElementById('qr-img-wechat').src = wechat;
        document.getElementById('qr-img-fps').src = fps;
    }

    function uploadPaymentQR(input, type) {
        if (!input.files || !input.files[0]) return;
        let formData = new FormData();
        formData.append("image", input.files[0]);
        alert("正在上傳全新收款 QR Code...");
        fetch("https://api.imgur.com/3/image", {
            method: "POST",
            headers: { Authorization: "Client-ID " + IMGUR_CLIENT_ID },
            body: formData
        })
        .then(res => res.json())
        .then(data => {
            if (data.success) {
                localStorage.setItem("lucky_qr_" + type, data.data.link);
                loadPaymentQR();
                switchPaymentQR();
                alert("收款二維碼更新成功！");
            } else { alert("上傳失敗。"); }
        }).catch(err => alert("上傳發生連線錯誤！"));
    }

    function changeHomePhoto() { document.getElementById('home-file-input').click(); }
    function handleHomePhotoUpload(input) { uploadToImgur(input, 'lucky_home_photo', 'home-hero-img'); }
    
    function changeNoticePhoto() { document.getElementById('notice-file-input').click(); }
    function handleNoticePhotoUpload(input) { uploadToImgur(input, 'lucky_notice_photo', 'notice-hero-img'); }

    function uploadToImgur(input, storageKey, elementId) {
        if (!input.files || !input.files[0]) return;
        let file = input.files[0];
        let formData = new FormData();
        formData.append("image", file);
        alert("正在上傳相片至雲端，請稍候...");
        fetch("https://api.imgur.com/3/image", {
            method: "POST",
            headers: { Authorization: "Client-ID " + IMGUR_CLIENT_ID },
            body: formData
        })
        .then(res => res.json())
        .then(data => {
            if (data.success) {
                let imgUrl = data.data.link;
                document.getElementById(elementId).src = imgUrl;
                localStorage.setItem(storageKey, imgUrl);
                alert("相片更新成功！請記得將最新的網頁代碼同步至 GitHub 保存。");
            } else {
                alert("上傳失敗，請稍後重試。");
            }
        }).catch(err => alert("連線發生錯誤！"));
    }

    document.getElementById("page-title").addEventListener("click", function() {
         showSection('home');
    });

    function showSection(section, titleText) {
        document.getElementById('home-section').style.display = 'none';
        document.getElementById('policy-section').style.display = 'none';
        document.getElementById('products-section').style.display = 'none';
        document.getElementById('contact-section').style.display = 'none';
        document.getElementById('cart-page-section').style.display = 'none';
        document.getElementById('checkout-page-section').style.display = 'none';
        
        document.getElementById('main-header-area').style.display = (section === 'cart' || section === 'checkout') ? 'none' : 'block';

        if (section === 'home') {
            document.getElementById('home-section').style.display = 'block';
            document.getElementById('main-title').innerText = "Welcome to Lucky.ccd_shop";
            document.getElementById('page-title').innerText = "Home ﹀";
        } 
        else if (section === 'policy') {
            document.getElementById('policy-section').style.display = 'block';
            document.getElementById('main-title').innerText = "";
            document.getElementById('page-title').innerText = titleText + " ﹀";
        }
        else if (section === 'products') {
            document.getElementById('products-section').style.display = 'block';
            document.getElementById('main-title').innerText = "";
        } 
        else if (section === 'contact') {
            document.getElementById('contact-section').style.display = 'block';
            document.getElementById('main-title').innerText = "";
            if(titleText) document.getElementById('page-title').innerText = titleText + " ﹀";
        }
        else if (section === 'cart') {
            document.getElementById('cart-page-section').style.display = 'block';
            setupCartPage();
        }
        else if (section === 'checkout') {
            document.getElementById('checkout-page-section').style.display = 'block';
            setupCheckoutPage();
        }
        window.scrollTo(0,0);
    }

    function filterProducts(type, val1, val2, titleText) {
        let grid = document.getElementById('product-grid');
        let cards = Array.from(grid.getElementsByClassName('card'));
        
        cards.forEach(card => {
            let b = card.getAttribute('data-brand');
            let p = parseFloat(card.getAttribute('data-price'));
            
            if (type === 'all') {
                card.style.display = 'block';
            } else if (type === 'brand') {
                card.style.display = (b === val1) ? 'block' : 'none';
            } else if (type === 'price') {
                card.style.display = (p >= val1 && p <= val2) ? 'block' : 'none';
            }
        });

        grid.innerHTML = "";
        cards.forEach(card => grid.appendChild(card));

        if (type === 'all') {
            document.getElementById('page-title').innerText = "Products ﹀";
        } else if (titleText) {
            document.getElementById('page-title').innerText = titleText + " ﹀";
        } else if (type === 'brand') {
            document.getElementById('page-title').innerText = val1 + " ﹀";
        }
    }

    function handleProductPhotoUpload(input) {
        if (!input.files || !input.files[0]) return;
        let file = input.files[0];
        let status = document.getElementById('upload-status');
        status.innerText = "正在上傳至雲端伺服器...";
        status.style.color = "orange";
        let formData = new FormData();
        formData.append("image", file);
        fetch("https://api.imgur.com/3/image", {
            method: "POST",
            headers: { Authorization: "Client-ID " + IMGUR_CLIENT_ID },
            body: formData
        })
        .then(res => res.json())
        .then(data => {
            if (data.success) {
                document.getElementById('adm-img').value = data.data.link;
                status.innerText = "✅ 相片上傳成功並已自動填寫網址！";
                status.style.color = "green";
            } else {
                status.innerText = "❌ 上傳失敗，請再試一次";
                status.style.color = "red";
            }
        }).catch(err => {
            status.innerText = "❌ 連線失敗";
            status.style.color = "red";
        });
    }

    function toggleSidebar() { document.getElementById('sidebar-menu').classList.toggle('active'); }
    function toggleSubMenu(id) { document.getElementById(id).classList.toggle('active'); }
    function toggleSearch() {
        let box = document.getElementById('search-box');
        box.style.display = (box.style.display === 'block') ? 'none' : 'block';
    }

    function sendContactForm() {
        const name = document.getElementById('ct-name').value;
        const email = document.getElementById('ct-email').value;
        const phone = document.getElementById('ct-phone').value;
        const comment = document.getElementById('ct-comment').value;
        if(!email) { alert("請填寫必填的 Email 欄位！"); return; }
        
        let text = `✉️【My Hidden Gem CCD - 聯絡客訴表單】✨\n\n• 姓名：${name || '未填'}\n• 電郵：${email}\n• 電話：${phone || '未填'}\n\n💬 留言：${comment || '無'}`;
        
        navigator.clipboard.writeText(text).then(() => {
            alert("聯絡表單資料已自動複製！請在跳轉後的 IG 聊天室直接「貼上」發送給我們唷！❤️");
            window.open(`https://www.instagram.com/${instagramUsername}/`, '_blank');
        }).catch(err => {
            alert("自動複製失敗，請手動截圖後前往 IG 聯絡我們！");
            window.open(`https://www.instagram.com/${instagramUsername}/`, '_blank');
        });
    }

    function searchProducts() {
        showSection('products');
        let query = document.getElementById('search-input').value.toLowerCase();
        let cards = document.querySelectorAll('.card');
        cards.forEach(card => {
            let name = card.querySelector('.name').innerText.toLowerCase();
            card.style.display = name.includes(query) ? 'block' : 'none';
        });
        document.getElementById('page-title').innerText = "Search Results ﹀";
    }

    // 登入功能
    function openLoginModal() { document.getElementById('login-modal').style.display = 'block'; backToEmailStep(); }
    function closeLoginModal() { document.getElementById('login-modal').style.display = 'none'; }
    
    function goToVerificationStep() {
        let email = document.getElementById('login-email').value;
        if (!email.includes('@')) { alert("請輸入正確的 Email 地址！"); return; }
        let btn = document.getElementById('email-submit-btn');
        btn.innerText = "Sending..."; btn.disabled = true;
        realSecurityCode = Math.floor(100000 + Math.random() * 900000).toString();
        
        emailjs.send(EMAILJS_SERVICE_ID, EMAILJS_TEMPLATE_ID, {
            to_email: email, verification_code: realSecurityCode 
        })
        .then(function() {
            document.getElementById('display-target-email').innerText = email;
            document.getElementById('login-step-email').style.display = 'none';
            document.getElementById('login-step-code').style.display = 'block';
            document.querySelectorAll('.code-input')[0].focus();
        }, function(error) {
            alert("郵件發送失敗！Error: " + JSON.stringify(error));
        }).finally(function() {
            btn.innerText = "Continue"; btn.disabled = false;
        });
    }

    function backToEmailStep() {
        document.getElementById('login-step-email').style.display = 'block';
        document.getElementById('login-step-code').style.display = 'none';
        document.querySelectorAll('.code-input').forEach(input => input.value = "");
    }

    document.querySelectorAll('.code-input').forEach((input, index) => {
        input.addEventListener('input', function() { moveCodeFocus(this, index); });
        input.addEventListener('keydown', function(e) { handleCodeBackspace(e, index); });
    });

    function moveCodeFocus(current, index) {
        if (current.value.length >= 1 && index < 5) document.querySelectorAll('.code-input')[index + 1].focus();
    }
    function handleCodeBackspace(e, index) {
        if (e.key === "Backspace" && !e.target.value && index > 0) document.querySelectorAll('.code-input')[index - 1].focus();
    }

    function verifyAndLogin() {
        let enteredCode = "";
        document.querySelectorAll('.code-input').forEach(input => enteredCode += input.value);
        if (enteredCode !== realSecurityCode || realSecurityCode === "") {
            alert("驗證碼不正確！"); return;
        }

        currentEmail = document.getElementById('login-email').value.trim();
        let saved = localStorage.getItem("cart_" + currentEmail);
        if (saved) { cart = JSON.parse(saved); renderCart(); } else { saveCartData(); }
        
        updateLoginStatusBar();
        checkAdminVisibility();
        closeLoginModal(); 
        showSection('cart');
    }

    function updateLoginStatusBar() {
        let bar = document.getElementById('page-login-status-bar');
        if (currentEmail) {
            if (currentEmail.toLowerCase() === ADMIN_EMAIL) {
                bar.innerHTML = `👑 已登入店主帳戶: <b>${currentEmail}</b> <button class="btn" style="padding:2px 8px; font-size:11px; margin-left:10px;" onclick="logout()">登出</button>`;
            } else {
                bar.innerHTML = `👤 已登入帳戶: <b>${currentEmail}</b> <button class="btn" style="padding:2px 8px; font-size:11px; margin-left:10px;" onclick="logout()">登出</button>`;
            }
        } else {
            bar.innerHTML = `💡 想要儲存購物車？ <button class="btn" style="padding:2px 8px; font-size:11px;" onclick="openLoginModal()">Login</button>`;
        }
    }

    function logout() {
        currentEmail = ""; cart = {}; renderCart();
        updateLoginStatusBar();
        checkAdminVisibility(); 
        showSection('home'); 
        alert("已成功登出！");
    }

    // 購物車功能核心
    function addToCart(name, price, imgUrl) {
        if (!imgUrl) imgUrl = "https://via.placeholder.com/400?text=Camera";
        if (!cart[name]) cart[name] = { price: price, qty: 1, img: imgUrl };
        else cart[name].qty++;
        saveCartData(); renderCart();
    }
    
    function changePageQty(name, amount) {
        if (!cart[name]) return;
        cart[name].qty += amount;
        if (cart[name].qty <= 0) delete cart[name];
        saveCartData(); renderCart(); setupCartPage();
    }
    
    function deletePageItem(name) { delete cart[name]; saveCartData(); renderCart(); setupCartPage(); }
    
    function renderCart() {
        let totalCount = 0; let totalPrice = 0;
        for (let key in cart) { totalCount += cart[key].qty; totalPrice += (cart[key].qty * cart[key].price); }
        document.getElementById('icon-cart-count').innerText = totalCount;
        document.getElementById('cart-total-price').innerText = totalPrice;
        
        let curDisplay = document.getElementById('cart-page-section').style.display;
        let curCheckoutDisplay = document.getElementById('checkout-page-section').style.display;
        if (totalCount > 0 && curDisplay !== 'block' && curCheckoutDisplay !== 'block') {
            document.getElementById('footer-bar').style.display = 'block';
        } else {
            document.getElementById('footer-bar').style.display = 'none';
        }
    }

    function setupCartPage() {
        document.getElementById('footer-bar').style.display = 'none';
        updateLoginStatusBar();
        let container = document.getElementById('page-cart-items-container');
        container.innerHTML = ""; 
        let totalPrice = 0; 
        let hasItems = false;
        
        for (let name in cart) {
            hasItems = true; 
            let itemTotal = cart[name].price * cart[name].qty;
            totalPrice += itemTotal;
            
            let itemDiv = document.createElement('div');
            itemDiv.className = 'custom-cart-item';
            itemDiv.innerHTML = `
                <img src="${cart[name].img}" alt="${name}">
                <div class="custom-cart-details">
                    <div class="custom-cart-name">${name}</div>
                    <div class="custom-cart-price">HK$${cart[name].price.toLocaleString()}.00</div>
                    <div class="custom-cart-actions">
                        <div class="qty-wrapper">
                            <button class="qty-btn" onclick="changePageQty('${name}', -1)">−</button>
                            <span class="qty-num">${cart[name].qty}</span>
                            <button class="qty-btn" onclick="changePageQty('${name}', 1)">+</button>
                        </div>
                        <button class="delete-icon-btn" onclick="deletePageItem('${name}')">🗑</button>
                    </div>
                </div>
                <div style="font-size: 15px; color: var(--main); font-weight: 500;">
                    HK$${itemTotal.toLocaleString()}.00
                </div>
            `;
            container.appendChild(itemDiv);
        }
        
        document.getElementById('page-cart-estimated-total').innerText = `HK$${totalPrice.toLocaleString()}.00`;
        
        if (hasItems) {
            document.getElementById('page-cart-summary-area').style.display = 'block';
            document.getElementById('page-empty-cart-view').style.display = 'none';
        } else {
            document.getElementById('page-cart-summary-area').style.display = 'none';
            document.getElementById('page-empty-cart-view').style.display = 'block';
        }
    }

    function setupCheckoutPage() {
        document.getElementById('footer-bar').style.display = 'none';
        let container = document.getElementById('checkout-products-summary');
        container.innerHTML = "";
        let totalPrice = 0;
        
        for (let name in cart) {
            let itemTotal = cart[name].price * cart[name].qty;
            totalPrice += itemTotal;
            
            let div = document.createElement('div');
            div.className = 'checkout-summary-item';
            div.innerHTML = `
                <span>${name} <b>(x${cart[name].qty})</b></span>
                <span>HK$${itemTotal.toLocaleString()}.00</span>
            `;
            container.appendChild(div);
        }
        
        let totalDiv = document.createElement('div');
        totalDiv.className = 'checkout-summary-item';
        totalDiv.style.borderTop = '1px dashed var(--main)';
        totalDiv.style.marginTop = '10px';
        totalDiv.style.paddingTop = '10px';
        totalDiv.style.fontWeight = 'bold';
        totalDiv.style.fontSize = '16px';
        totalDiv.style.color = 'red';
        totalDiv.innerHTML = `
            <span>總計金額 (Total)</span>
            <span>HK$${totalPrice.toLocaleString()}.00</span>
        `;
        container.appendChild(totalDiv);
        
        switchPaymentQR();
    }

    function switchPaymentQR() {
        let method = document.getElementById('payment-method-select').value;
        document.getElementById('qr-img-alipay').style.display = 'none';
        document.getElementById('qr-img-wechat').style.display = 'none';
        document.getElementById('qr-img-fps').style.display = 'none';
        
        if(method === 'alipay') document.getElementById('qr-img-alipay').style.display = 'block';
        if(method === 'wechat') document.getElementById('qr-img-wechat').style.display = 'block';
        if(method === 'fps') document.getElementById('qr-img-fps').style.display = 'block';
    }
    
    function saveCartData() { if (currentEmail) localStorage.setItem("cart_" + currentEmail, JSON.stringify(cart)); }

    // 提交訂單：驗證順豐資料並複製格式化文字到剪貼簿後跳轉 IG (優化版跳轉與安全彈窗機制)
    function submitOrder() {
        const igCamera = document.getElementById('checkout-ig-camera').value.trim();
        const name = document.getElementById('sf-name').value.trim();
        const phone = document.getElementById('sf-phone').value.trim();
        const address = document.getElementById('sf-address').value.trim();
        const paymentMethod = document.getElementById('payment-method-select').value.toUpperCase();

        // 1. 表單資料驗證
        if (!igCamera) { 
            alert("⚠️ 錯誤：請填寫【IG Name + 相機型號】以供核對過數！"); 
            document.getElementById('checkout-ig-camera').focus();
            return; 
        }
        if (!name) { 
            alert("⚠️ 錯誤：請填寫順豐收件人的【姓名】！"); 
            document.getElementById('sf-name').focus();
            return; 
        }
        if (!phone) { 
            alert("⚠️ 錯誤：請填寫順豐收件人的【聯絡電話】！"); 
            document.getElementById('sf-phone').focus();
            return; 
        }
        if (!address) { 
            alert("⚠️ 錯誤：請填寫【順豐收件地址/網點編號】！"); 
            document.getElementById('sf-address').focus();
            return; 
        }
        
        // 2. 打包文字資料
        let orderList = ""; let totalPrice = 0;
        for (let key in cart) { 
            orderList += `- ${key} x ${cart[key].qty} (HK$${(cart[key].price * cart[key].qty).toLocaleString()})\n`; 
            totalPrice += (cart[key].qty * cart[key].price); 
        }
        
        let text = `✨【My Hidden Gem CCD 新訂單】✨\n`;
        if(currentEmail) text += `會員帳號：${currentEmail}\n`;
        text += `📱 買家資料：${igCamera}\n`;
        text += `🛒 購買項目：\n${orderList}💰 總金額：HK$${totalPrice.toLocaleString()}.00\n`;
        text += `💳 付款方式：${paymentMethod}\n\n`;
        text += `📦 順豐資料：\n• 姓名：${name}\n• 電話：${phone}\n• 地址：${address}`;
        
        // 3. 執行自動複製與多重安全跳轉
        navigator.clipboard.writeText(text).then(() => {
            let igWebUrl = `https://www.instagram.com/${instagramUsername}/`;
            
            // 🌟 這裡已經修正為使用反單引號 ` 來讓變數正確顯示！
            let confirmGo = confirm(`🎉 訂單資料已成功複製到剪貼簿！\n\n系統接著會為您開啟 Instagram 專頁。\n進入 IG 聊天室後「直接貼上並送出文字」，並發送付款截圖即可完成訂購！❤️\n\n(若系統未自動跳轉，請手動前往 IG 搜尋：${instagramUsername})`);
            
            if (confirmGo) {
                // 嘗試使用通用多重跳轉協議
                window.location.href = `instagram://user?username=${instagramUsername}`;
                
                // 保底防護線：在全新分頁強制開啟 IG 官方網頁版 / 個人檔案
                setTimeout(function() {
                    window.open(igWebUrl, '_blank');
                }, 400);
            }
        }).catch(err => {
            alert("自動複製失敗，請手動將結帳畫面完整截圖，並前往 IG 私訊我們完成訂購！");
            window.open(`https://www.instagram.com/${instagramUsername}/`, '_blank');
        });
    }

    function adminAddProduct() {
        const brand = document.getElementById('adm-brand').value;
        const img = document.getElementById('adm-img').value;
        const name = document.getElementById('adm-name').value;
        const price = document.getElementById('adm-price').value;
        if (!img || !name || !price) { alert("請填妥所有商品資料！"); return; }
        
        const htmlCode = `<div class="card" data-brand="${brand}" data-price="${price}" data-img="${img}"><div class="brand-tag">${brand}</div><img src="${img}"><div class="name">${name}</div><div class="price">HK$${parseFloat(price).toFixed(2)}</div><button class="btn" onclick="addToCart('${name}', ${price}, '${img}')">Add to cart</button></div>`;
        document.getElementById('adm-output').value = htmlCode;
        let tempContainer = document.createElement('div'); tempContainer.innerHTML = htmlCode;
        document.getElementById('product-grid').appendChild(tempContainer.firstElementChild);
        
        document.getElementById('adm-file-input').value = ""; document.getElementById('adm-img').value = "";
        document.getElementById('upload-status').innerText = "未選擇檔案"; document.getElementById('upload-status').style.color = "#666";
        alert("預覽成功！請記得複製最下方的網頁代碼貼回 GitHub 保存。");
    }
</script>
</body>
</html>

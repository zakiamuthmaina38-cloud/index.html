<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AURELIA — Butik Aksesoris Modern & Elegan</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts: Cormorant Garamond & Plus Jakarta Sans -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,500;0,600;0,700;1,500&family=Plus+Jakarta+Sans:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        serif: ['"Cormorant Garamond"', 'serif'],
                        sans: ['"Plus Jakarta Sans"', 'sans-serif'],
                    },
                    colors: {
                        ivory: '#FAF8F5',
                        sand: '#F2EDE4',
                        champagne: '#E8DFD1',
                        charcoal: '#1C1917',
                        gold: {
                            300: '#E5C76B',
                            400: '#D4AF37',
                            500: '#B8860B',
                            600: '#9A6E08',
                        }
                    }
                }
            }
        }
    </script>
    <style>
        body {
            background-color: #FAF8F5;
            color: #1C1917;
            font-family: 'Plus Jakarta Sans', sans-serif;
            overflow-x: hidden;
        }
        .font-serif-display {
            font-family: 'Cormorant Garamond', serif;
        }
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #F2EDE4;
        }
        ::-webkit-scrollbar-thumb {
            background: #D6CEC2;
            border-radius: 9999px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #B8860B;
        }
        .glass-nav {
            background: rgba(250, 248, 245, 0.9);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
        }
        .product-card:hover .product-img {
            transform: scale(1.06);
        }
        .no-scrollbar::-webkit-scrollbar {
            display: none;
        }
        .no-scrollbar {
            -ms-overflow-style: none;
            scrollbar-width: none;
        }
    </style>
</head>
<body class="antialiased selection:bg-amber-200 selection:text-stone-900 min-h-screen flex flex-col">

    <!-- Toast Notification Container -->
    <div id="toast-container" class="fixed bottom-5 right-5 z-[60] flex flex-col gap-2.5 max-w-sm w-full px-4 sm:px-0 pointer-events-none"></div>

    <!-- Navigation Bar -->
    <header class="sticky top-0 z-40 glass-nav border-b border-stone-200/80 transition-all">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex items-center justify-between h-16 sm:h-20 gap-2">
                <!-- Brand Logo -->
                <div class="flex items-center gap-2.5 cursor-pointer shrink-0" onclick="switchView('store')">
                    <div class="w-9 h-9 sm:w-10 sm:h-10 rounded-full bg-stone-900 text-amber-400 flex items-center justify-center shadow-sm">
                        <i data-lucide="sparkles" class="w-4 h-4 sm:w-5 sm:h-5"></i>
                    </div>
                    <div>
                        <span class="font-serif-display text-2xl sm:text-3xl font-bold tracking-wider text-stone-900 block leading-none">AURELIA</span>
                        <span class="text-[9px] sm:text-[10px] tracking-[0.25em] uppercase text-stone-500 block mt-0.5">Fine Accessories</span>
                    </div>
                </div>

                <!-- Database Status Pill (Desktop) -->
                <div id="header-db-status" class="hidden lg:flex items-center gap-2 px-3.5 py-1.5 rounded-full bg-emerald-50 border border-emerald-200/80 text-emerald-800 text-xs font-medium">
                    <span class="w-2 h-2 rounded-full bg-emerald-500 animate-pulse"></span>
                    <span id="header-db-text">Menghubungkan Database...</span>
                </div>

                <!-- Mode Switcher & Cart -->
                <div class="flex items-center gap-2 sm:gap-3">
                    <!-- View Toggle: Toko vs Admin -->
                    <div class="bg-stone-200/80 p-1 rounded-full flex items-center text-xs sm:text-sm font-medium">
                        <button type="button" id="nav-store-btn" onclick="switchView('store')" class="px-3 sm:px-4 py-1.5 rounded-full bg-stone-900 text-white shadow-sm transition-all flex items-center gap-1.5">
                            <i data-lucide="store" class="w-3.5 h-3.5"></i>
                            <span>Katalog</span>
                        </button>
                        <button type="button" id="nav-admin-btn" onclick="switchView('admin')" class="px-3 sm:px-4 py-1.5 rounded-full text-stone-600 hover:text-stone-900 transition-all flex items-center gap-1.5">
                            <i data-lucide="shield-check" class="w-3.5 h-3.5"></i>
                            <span>Panel Admin</span>
                        </button>
                    </div>

                    <!-- Cart Button -->
                    <button type="button" onclick="toggleCart(true)" class="relative p-2.5 sm:px-4 sm:py-2 rounded-full bg-amber-600 hover:bg-amber-700 text-white transition-all shadow-sm flex items-center gap-2">
                        <i data-lucide="shopping-bag" class="w-4 h-4"></i>
                        <span class="hidden sm:inline text-sm font-medium">Keranjang</span>
                        <span id="cart-badge" class="bg-stone-900 text-amber-300 text-xs font-bold w-5 h-5 rounded-full flex items-center justify-center">0</span>
                    </button>
                </div>
            </div>
        </div>
    </header>

    <!-- ==================== VIEW 1: STOREFRONT (KATALOG PELANGGAN) ==================== -->
    <main id="store-view" class="block flex-1">
        <!-- Hero Banner -->
        <section class="relative bg-stone-900 text-ivory overflow-hidden">
            <div class="absolute inset-0 opacity-35">
                <img src="https://images.unsplash.com/photo-1515562141207-7a88fb7ce338?auto=format&fit=crop&w=1600&q=80" 
                     alt="Koleksi Perhiasan Aurelia" 
                     class="w-full h-full object-cover object-center"
                     onerror="this.src='https://placehold.co/1600x600/1C1917/D4AF37?text=AURELIA+BOUTIQUE'">
            </div>
            <div class="absolute inset-0 bg-gradient-to-r from-stone-950 via-stone-900/85 to-transparent"></div>
            
            <div class="relative max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-14 sm:py-24">
                <div class="max-w-xl">
                    <span class="inline-block px-3 py-1 rounded-full bg-amber-500/20 border border-amber-400/30 text-amber-300 text-xs font-medium tracking-widest uppercase mb-4">
                        Koleksi Eksklusif 2026
                    </span>
                    <h1 class="font-serif-display text-4xl sm:text-6xl font-bold leading-tight mb-4">
                        Sentuhan Kemewahan di Setiap Detail Gaya Anda.
                    </h1>
                    <p class="text-stone-300 text-sm sm:text-base leading-relaxed mb-8">
                        Temukan kurasi eksklusif kalung, gelang, cincin, anting, dan jam tangan klasik yang dirancang dengan presisi tinggi untuk menyempurnakan momen istimewa.
                    </p>
                    <a href="#katalog-section" class="inline-flex items-center gap-2 px-6 py-3.5 rounded-full bg-amber-500 hover:bg-amber-400 text-stone-950 font-semibold text-sm transition-all shadow-lg">
                        <span>Jelajahi Koleksi</span>
                        <i data-lucide="arrow-down-right" class="w-4 h-4"></i>
                    </a>
                </div>
            </div>
        </section>

        <!-- Filter & Search Bar -->
        <section id="katalog-section" class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 pt-10 pb-6">
            <div class="flex flex-col md:flex-row md:items-center justify-between gap-4 pb-6 border-b border-stone-200">
                <!-- Category Pills -->
                <div id="category-filters" class="flex items-center gap-2 overflow-x-auto pb-2 md:pb-0 no-scrollbar">
                    <button type="button" onclick="setCategory('Semua')" data-cat="Semua" class="cat-btn active px-4 py-2 rounded-full text-sm font-medium bg-stone-900 text-white transition-all whitespace-nowrap">Semua</button>
                    <button type="button" onclick="setCategory('Kalung')" data-cat="Kalung" class="cat-btn px-4 py-2 rounded-full text-sm font-medium bg-sand text-stone-700 hover:bg-stone-200 transition-all whitespace-nowrap">Kalung</button>
                    <button type="button" onclick="setCategory('Gelang')" data-cat="Gelang" class="cat-btn px-4 py-2 rounded-full text-sm font-medium bg-sand text-stone-700 hover:bg-stone-200 transition-all whitespace-nowrap">Gelang</button>
                    <button type="button" onclick="setCategory('Cincin')" data-cat="Cincin" class="cat-btn px-4 py-2 rounded-full text-sm font-medium bg-sand text-stone-700 hover:bg-stone-200 transition-all whitespace-nowrap">Cincin</button>
                    <button type="button" onclick="setCategory('Anting')" data-cat="Anting" class="cat-btn px-4 py-2 rounded-full text-sm font-medium bg-sand text-stone-700 hover:bg-stone-200 transition-all whitespace-nowrap">Anting</button>
                    <button type="button" onclick="setCategory('Jam Tangan')" data-cat="Jam Tangan" class="cat-btn px-4 py-2 rounded-full text-sm font-medium bg-sand text-stone-700 hover:bg-stone-200 transition-all whitespace-nowrap">Jam Tangan</button>
                </div>

                <!-- Search Input -->
                <div class="relative w-full md:w-72">
                    <i data-lucide="search" class="w-4 h-4 text-stone-400 absolute left-3.5 top-1/2 -translate-y-1/2"></i>
                    <input type="text" id="search-input" oninput="renderProducts()" placeholder="Cari aksesoris..." 
                           class="w-full pl-10 pr-4 py-2.5 rounded-full bg-white border border-stone-200 text-sm focus:outline-none focus:border-amber-500 focus:ring-2 focus:ring-amber-500/20 transition-all">
                </div>
            </div>
        </section>

        <!-- Product Grid -->
        <section class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 pb-20">
            <!-- Loading State -->
            <div id="products-loading" class="py-16 text-center">
                <div class="inline-block w-8 h-8 border-4 border-amber-500 border-t-transparent rounded-full animate-spin mb-3"></div>
                <p class="text-stone-500 text-sm">Memuat koleksi aksesoris...</p>
            </div>

            <!-- Empty State -->
            <div id="products-empty" class="hidden py-16 text-center bg-white rounded-3xl border border-stone-200/60 p-8">
                <i data-lucide="gem" class="w-12 h-12 text-stone-300 mx-auto mb-3"></i>
                <h3 class="font-serif-display text-2xl font-bold text-stone-800 mb-1">Aksesoris Tidak Ditemukan</h3>
                <p class="text-stone-500 text-sm mb-4">Coba ubah kata kunci pencarian atau pilih kategori lainnya.</p>
                <button type="button" onclick="resetFilters()" class="px-5 py-2 rounded-full bg-stone-900 text-white text-sm font-medium hover:bg-stone-800 transition-all">Reset Filter</button>
            </div>

            <!-- Grid Container -->
            <div id="product-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
                <!-- Product cards injected via JS -->
            </div>
        </section>
    </main>

    <!-- ==================== VIEW 2: ADMIN PANEL (CRUD PRODUK & PESANAN) ==================== -->
    <main id="admin-view" class="hidden flex-1 max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-8 sm:py-12">
        <!-- Admin Header -->
        <div class="flex flex-col lg:flex-row lg:items-center justify-between gap-4 mb-8 bg-stone-900 text-ivory p-6 sm:p-8 rounded-3xl shadow-md">
            <div>
                <div class="flex flex-wrap items-center gap-2 mb-2">
                    <span id="admin-db-badge" class="inline-flex items-center gap-1.5 px-3 py-1 rounded-full bg-emerald-500/20 border border-emerald-400/30 text-emerald-300 text-xs font-semibold">
                        <span class="w-2 h-2 rounded-full bg-emerald-400"></span>
                        <span id="admin-db-text">Database Aktif</span>
                    </span>
                </div>
                <h1 class="font-serif-display text-3xl sm:text-4xl font-bold">Panel Admin Aurelia</h1>
                <p class="text-stone-400 text-sm mt-1">Kelola katalog produk aksesoris, stok barang, dan pantau pesanan pelanggan.</p>
            </div>
            <div class="flex flex-wrap items-center gap-3">
                <button type="button" onclick="confirmResetDatabase()" class="px-4 py-2.5 rounded-full bg-stone-800 hover:bg-stone-700 text-stone-200 border border-stone-700 font-medium text-xs sm:text-sm transition-all flex items-center gap-2">
                    <i data-lucide="rotate-ccw" class="w-4 h-4 text-amber-400"></i>
                    <span>Reset Data Awal</span>
                </button>
                <button type="button" onclick="openProductModal()" class="px-5 py-2.5 rounded-full bg-amber-500 hover:bg-amber-400 text-stone-950 font-semibold text-xs sm:text-sm transition-all flex items-center gap-2 shadow-sm">
                    <i data-lucide="plus-circle" class="w-4 h-4"></i>
                    <span>Tambah Produk</span>
                </button>
            </div>
        </div>

        <!-- Admin Stats Cards -->
        <div class="grid grid-cols-2 lg:grid-cols-4 gap-4 mb-8">
            <div class="bg-white p-5 rounded-2xl border border-stone-200/80 shadow-sm">
                <p class="text-xs text-stone-500 font-medium uppercase">Total Produk</p>
                <p id="stat-total-products" class="font-serif-display text-3xl font-bold text-stone-900 mt-1">0</p>
            </div>
            <div class="bg-white p-5 rounded-2xl border border-stone-200/80 shadow-sm">
                <p class="text-xs text-stone-500 font-medium uppercase">Total Stok Barang</p>
                <p id="stat-total-stock" class="font-serif-display text-3xl font-bold text-stone-900 mt-1">0</p>
            </div>
            <div class="bg-white p-5 rounded-2xl border border-stone-200/80 shadow-sm">
                <p class="text-xs text-stone-500 font-medium uppercase">Total Pesanan</p>
                <p id="stat-total-orders" class="font-serif-display text-3xl font-bold text-amber-600 mt-1">0</p>
            </div>
            <div class="bg-white p-5 rounded-2xl border border-stone-200/80 shadow-sm">
                <p class="text-xs text-stone-500 font-medium uppercase">Estimasi Pendapatan</p>
                <p id="stat-revenue" class="font-serif-display text-2xl sm:text-3xl font-bold text-emerald-700 mt-1">Rp 0</p>
            </div>
        </div>

        <!-- Admin Sub-Tabs -->
        <div class="flex items-center gap-2 border-b border-stone-200 mb-6 overflow-x-auto no-scrollbar">
            <button type="button" id="tab-admin-products" onclick="switchAdminTab('products')" class="px-5 py-3 text-sm font-semibold border-b-2 border-stone-900 text-stone-900 transition-all flex items-center gap-2 whitespace-nowrap">
                <i data-lucide="package" class="w-4 h-4"></i>
                <span>Kelola Produk</span>
            </button>
            <button type="button" id="tab-admin-orders" onclick="switchAdminTab('orders')" class="px-5 py-3 text-sm font-medium border-b-2 border-transparent text-stone-500 hover:text-stone-900 transition-all flex items-center gap-2 whitespace-nowrap">
                <i data-lucide="receipt" class="w-4 h-4"></i>
                <span>Kelola Pesanan</span>
                <span id="admin-order-badge" class="bg-amber-100 text-amber-800 text-xs font-bold px-2 py-0.5 rounded-full">0</span>
            </button>
        </div>

        <!-- Admin Tab 1: Products Table -->
        <div id="admin-products-section" class="bg-white rounded-3xl border border-stone-200/80 shadow-sm overflow-hidden">
            <div class="overflow-x-auto">
                <table class="w-full text-left border-collapse">
                    <thead>
                        <tr class="bg-sand/60 text-stone-600 text-xs uppercase tracking-wider border-b border-stone-200">
                            <th class="py-4 px-6 font-semibold">Produk</th>
                            <th class="py-4 px-6 font-semibold">Kategori</th>
                            <th class="py-4 px-6 font-semibold">Harga</th>
                            <th class="py-4 px-6 font-semibold">Stok</th>
                            <th class="py-4 px-6 font-semibold text-right">Aksi</th>
                        </tr>
                    </thead>
                    <tbody id="admin-products-tbody" class="divide-y divide-stone-100 text-sm">
                        <!-- Admin product rows injected via JS -->
                    </tbody>
                </table>
            </div>
        </div>

        <!-- Admin Tab 2: Orders List -->
        <div id="admin-orders-section" class="hidden space-y-4">
            <div id="admin-orders-list" class="grid grid-cols-1 md:grid-cols-2 gap-4">
                <!-- Admin order cards injected via JS -->
            </div>
        </div>
    </main>

    <!-- Footer -->
    <footer class="bg-stone-900 text-stone-400 py-10 border-t border-stone-800 mt-auto">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 flex flex-col sm:flex-row items-center justify-between gap-4">
            <div class="flex items-center gap-2">
                <span class="font-serif-display text-2xl font-bold text-ivory tracking-wider">AURELIA</span>
                <span class="text-xs text-stone-500">© 2026 Butik Aksesoris Modern</span>
            </div>
            <p class="text-xs text-stone-500 text-center sm:text-right">Kualitas Terjamin • Pengiriman Seluruh Indonesia • Pembayaran Aman</p>
        </div>
    </footer>

    <!-- ==================== DRAWER: KERANJANG BELANJA & CHECKOUT ==================== -->
    <div id="cart-drawer" class="fixed inset-0 z-50 hidden">
        <!-- Backdrop -->
        <div onclick="toggleCart(false)" class="absolute inset-0 bg-stone-950/60 backdrop-blur-xs transition-opacity"></div>
        
        <!-- Drawer Panel -->
        <div class="absolute right-0 top-0 bottom-0 w-full max-w-md bg-ivory shadow-2xl flex flex-col h-full">
            <!-- Drawer Header -->
            <div class="px-6 py-5 bg-white border-b border-stone-200 flex items-center justify-between">
                <div class="flex items-center gap-2.5">
                    <i data-lucide="shopping-bag" class="w-5 h-5 text-amber-600"></i>
                    <h2 class="font-serif-display text-2xl font-bold text-stone-900">Keranjang Belanja</h2>
                </div>
                <button type="button" onclick="toggleCart(false)" class="p-2 rounded-full hover:bg-stone-100 text-stone-500 transition-colors">
                    <i data-lucide="x" class="w-5 h-5"></i>
                </button>
            </div>

            <!-- Step 1: Cart Items View -->
            <div id="cart-items-step" class="flex-1 flex flex-col overflow-hidden">
                <div id="cart-items-container" class="flex-1 overflow-y-auto p-6 space-y-4">
                    <!-- Cart items injected via JS -->
                </div>

                <!-- Empty Cart Message -->
                <div id="empty-cart-msg" class="hidden flex-1 flex flex-col items-center justify-center p-8 text-center">
                    <div class="w-16 h-16 rounded-full bg-sand flex items-center justify-center text-stone-400 mb-4">
                        <i data-lucide="shopping-bag" class="w-8 h-8"></i>
                    </div>
                    <h3 class="font-serif-display text-2xl font-bold text-stone-800 mb-1">Keranjang Masih Kosong</h3>
                    <p class="text-sm text-stone-500 mb-6">Pilih aksesoris favorit Anda untuk melengkapi penampilan hari ini.</p>
                    <button type="button" onclick="toggleCart(false)" class="px-6 py-2.5 rounded-full bg-stone-900 text-white text-sm font-medium hover:bg-stone-800 transition-all">
                        Mulai Belanja
                    </button>
                </div>

                <!-- Cart Footer -->
                <div id="cart-footer" class="p-6 bg-white border-t border-stone-200 space-y-4">
                    <div class="flex items-center justify-between text-sm text-stone-500">
                        <span>Subtotal</span>
                        <span id="cart-subtotal" class="font-semibold text-stone-900">Rp 0</span>
                    </div>
                    <div class="flex items-center justify-between text-base font-bold text-stone-900 pt-2 border-t border-stone-100">
                        <span>Total Pembayaran</span>
                        <span id="cart-total" class="text-amber-700 text-lg">Rp 0</span>
                    </div>
                    <button type="button" onclick="showCheckoutStep(true)" class="w-full py-3.5 rounded-full bg-stone-900 hover:bg-amber-600 text-white font-semibold text-sm transition-all shadow-md flex items-center justify-center gap-2">
                        <span>Lanjut ke Checkout</span>
                        <i data-lucide="arrow-right" class="w-4 h-4"></i>
                    </button>
                </div>
            </div>

            <!-- Step 2: Checkout Form View -->
            <div id="checkout-form-step" class="hidden flex-1 flex flex-col overflow-hidden">
                <div class="px-6 py-3 bg-sand/70 border-b border-stone-200 flex items-center gap-2">
                    <button type="button" onclick="showCheckoutStep(false)" class="text-xs font-semibold text-stone-600 hover:text-stone-900 flex items-center gap-1">
                        <i data-lucide="arrow-left" class="w-3.5 h-3.5"></i>
                        <span>Kembali ke Keranjang</span>
                    </button>
                </div>

                <form id="checkout-form" novalidate class="flex-1 overflow-y-auto p-6 space-y-4">
                    <div>
                        <label class="block text-xs font-semibold uppercase tracking-wider text-stone-600 mb-1.5">Nama Lengkap Penerima *</label>
                        <input type="text" id="cust-name" placeholder="Contoh: Clara Wijaya" 
                               class="w-full px-4 py-2.5 rounded-xl bg-white border border-stone-200 text-sm focus:outline-none focus:border-amber-500">
                    </div>

                    <div>
                        <label class="block text-xs font-semibold uppercase tracking-wider text-stone-600 mb-1.5">Nomor WhatsApp *</label>
                        <input type="tel" id="cust-phone" placeholder="Contoh: 081234567890" 
                               class="w-full px-4 py-2.5 rounded-xl bg-white border border-stone-200 text-sm focus:outline-none focus:border-amber-500">
                    </div>

                    <div>
                        <label class="block text-xs font-semibold uppercase tracking-wider text-stone-600 mb-1.5">Alamat Lengkap Pengiriman *</label>
                        <textarea id="cust-address" rows="3" placeholder="Nama jalan, nomor rumah, kecamatan, kota, kode pos..." 
                                  class="w-full px-4 py-2.5 rounded-xl bg-white border border-stone-200 text-sm focus:outline-none focus:border-amber-500"></textarea>
                    </div>

                    <div>
                        <label class="block text-xs font-semibold uppercase tracking-wider text-stone-600 mb-1.5">Metode Pembayaran *</label>
                        <select id="cust-payment" class="w-full px-4 py-2.5 rounded-xl bg-white border border-stone-200 text-sm focus:outline-none focus:border-amber-500">
                            <option value="Transfer Bank (BCA / Mandiri)">Transfer Bank (BCA / Mandiri)</option>
                            <option value="QRIS (GoPay, OVO, Dana, ShopeePay)">QRIS (GoPay, OVO, Dana, ShopeePay)</option>
                            <option value="COD (Bayar di Tempat)">COD (Bayar di Tempat)</option>
                        </select>
                    </div>

                    <!-- Ringkasan Singkat -->
                    <div class="p-4 rounded-2xl bg-sand/60 border border-stone-200/80 space-y-1.5">
                        <div class="flex justify-between text-xs text-stone-600">
                            <span>Jumlah Item:</span>
                            <span id="checkout-item-count" class="font-semibold">0 produk</span>
                        </div>
                        <div class="flex justify-between text-sm font-bold text-stone-900">
                            <span>Total Tagihan:</span>
                            <span id="checkout-total-display" class="text-amber-700">Rp 0</span>
                        </div>
                    </div>

                    <button type="submit" id="submit-order-btn" class="w-full py-3.5 rounded-full bg-amber-600 hover:bg-amber-700 text-white font-semibold text-sm transition-all shadow-md flex items-center justify-center gap-2 mt-2">
                        <i data-lucide="check-circle-2" class="w-4 h-4"></i>
                        <span>Buat Pesanan Sekarang</span>
                    </button>
                </form>
            </div>
        </div>
    </div>

    <!-- ==================== MODAL: TAMBAH / EDIT PRODUK (ADMIN) ==================== -->
    <div id="product-modal" class="fixed inset-0 z-50 hidden flex items-center justify-center p-4">
        <div onclick="closeProductModal()" class="absolute inset-0 bg-stone-950/60 backdrop-blur-xs"></div>
        <div class="relative w-full max-w-lg bg-ivory rounded-3xl shadow-2xl border border-stone-200 overflow-hidden z-10">
            <div class="px-6 py-4 bg-white border-b border-stone-200 flex items-center justify-between">
                <h3 id="product-modal-title" class="font-serif-display text-2xl font-bold text-stone-900">Tambah Produk Baru</h3>
                <button type="button" onclick="closeProductModal()" class="p-1.5 rounded-full hover:bg-stone-100 text-stone-500">
                    <i data-lucide="x" class="w-5 h-5"></i>
                </button>
            </div>

            <form id="product-form" novalidate class="p-6 space-y-4 max-h-[82vh] overflow-y-auto">
                <input type="hidden" id="prod-id">
                
                <!-- Preview Gambar Produk -->
                <div class="flex items-center gap-4 p-3.5 bg-white rounded-2xl border border-stone-200/80">
                    <img id="prod-img-preview" 
                         src="https://images.unsplash.com/photo-1515562141207-7a88fb7ce338?auto=format&fit=crop&w=400&q=80" 
                         alt="Preview Produk" 
                         class="w-20 h-20 rounded-xl object-cover bg-sand border border-stone-200 shrink-0">
                    <div class="flex-1 min-w-0">
                        <p class="text-xs font-semibold text-stone-800 mb-1">Foto Produk Aksesoris</p>
                        <p class="text-[11px] text-stone-500 mb-2.5">Unggah foto dari galeri HP/Laptop atau tempel link URL gambar di bawah.</p>
                        <label class="inline-flex items-center gap-1.5 px-3.5 py-1.5 rounded-full bg-stone-900 hover:bg-amber-600 text-white text-xs font-medium cursor-pointer transition-colors">
                            <i data-lucide="upload" class="w-3.5 h-3.5"></i>
                            <span>Pilih Foto dari Perangkat</span>
                            <input type="file" id="prod-file-input" accept="image/*" class="hidden">
                        </label>
                    </div>
                </div>

                <div>
                    <label class="block text-xs font-semibold uppercase tracking-wider text-stone-600 mb-1">Nama Aksesoris *</label>
                    <input type="text" id="prod-name" placeholder="Contoh: Kalung Liontin Mutiara Emas" 
                           class="w-full px-4 py-2.5 rounded-xl bg-white border border-stone-200 text-sm focus:outline-none focus:border-amber-500">
                </div>

                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                    <div>
                        <label class="block text-xs font-semibold uppercase tracking-wider text-stone-600 mb-1">Kategori *</label>
                        <select id="prod-category" class="w-full px-4 py-2.5 rounded-xl bg-white border border-stone-200 text-sm focus:outline-none focus:border-amber-500">
                            <option value="Kalung">Kalung</option>
                            <option value="Gelang">Gelang</option>
                            <option value="Cincin">Cincin</option>
                            <option value="Anting">Anting</option>
                            <option value="Jam Tangan">Jam Tangan</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-xs font-semibold uppercase tracking-wider text-stone-600 mb-1">Harga (Rp) *</label>
                        <input type="number" id="prod-price" placeholder="250000" 
                               class="w-full px-4 py-2.5 rounded-xl bg-white border border-stone-200 text-sm focus:outline-none focus:border-amber-500">
                    </div>
                </div>

                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                    <div>
                        <label class="block text-xs font-semibold uppercase tracking-wider text-stone-600 mb-1">Stok Barang *</label>
                        <input type="number" id="prod-stock" placeholder="15" 
                               class="w-full px-4 py-2.5 rounded-xl bg-white border border-stone-200 text-sm focus:outline-none focus:border-amber-500">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold uppercase tracking-wider text-stone-600 mb-1">URL Gambar (Opsional)</label>
                        <input type="text" id="prod-image" placeholder="https://..." 
                               class="w-full px-4 py-2.5 rounded-xl bg-white border border-stone-200 text-sm focus:outline-none focus:border-amber-500">
                    </div>
                </div>

                <div>
                    <label class="block text-xs font-semibold uppercase tracking-wider text-stone-600 mb-1">Deskripsi Singkat</label>
                    <textarea id="prod-desc" rows="2" placeholder="Detail bahan, ukuran, atau keunggulan produk..." 
                              class="w-full px-4 py-2.5 rounded-xl bg-white border border-stone-200 text-sm focus:outline-none focus:border-amber-500"></textarea>
                </div>

                <div class="flex items-center justify-end gap-3 pt-3 border-t border-stone-200">
                    <button type="button" onclick="closeProductModal()" class="px-5 py-2.5 rounded-full bg-stone-200 hover:bg-stone-300 text-stone-800 text-sm font-medium transition-all">Batal</button>
                    <button type="submit" id="save-product-btn" class="px-6 py-2.5 rounded-full bg-stone-900 hover:bg-amber-600 text-white text-sm font-semibold transition-all shadow-sm flex items-center gap-2">
                        <i data-lucide="save" class="w-4 h-4"></i>
                        <span>Simpan Produk</span>
                    </button>
                </div>
            </form>
        </div>
    </div>

    <!-- ==================== MODAL: KONFIRMASI KUSTOM (HAPUS / RESET) ==================== -->
    <div id="confirm-modal" class="fixed inset-0 z-50 hidden flex items-center justify-center p-4">
        <div onclick="closeConfirmModal()" class="absolute inset-0 bg-stone-950/60 backdrop-blur-xs"></div>
        <div class="relative w-full max-w-sm bg-white rounded-3xl shadow-2xl p-6 text-center z-10">
            <div id="confirm-modal-icon" class="w-12 h-12 rounded-full bg-red-100 text-red-600 flex items-center justify-center mx-auto mb-4">
                <i data-lucide="alert-triangle" class="w-6 h-6"></i>
            </div>
            <h3 id="confirm-modal-title" class="font-serif-display text-2xl font-bold text-stone-900 mb-1">Konfirmasi</h3>
            <p id="confirm-modal-text" class="text-sm text-stone-500 mb-6">Apakah Anda yakin ingin melanjutkan tindakan ini?</p>
            <div class="flex items-center justify-center gap-3">
                <button type="button" onclick="closeConfirmModal()" class="flex-1 py-2.5 rounded-full bg-stone-100 hover:bg-stone-200 text-stone-800 text-sm font-medium transition-colors">Batal</button>
                <button type="button" id="confirm-modal-action-btn" class="flex-1 py-2.5 rounded-full bg-red-600 hover:bg-red-700 text-white text-sm font-semibold transition-colors">Ya, Lanjutkan</button>
            </div>
        </div>
    </div>

    <!-- ==================== MODAL: SUKSES ORDER ==================== -->
    <div id="order-success-modal" class="fixed inset-0 z-50 hidden flex items-center justify-center p-4">
        <div onclick="closeSuccessModal()" class="absolute inset-0 bg-stone-950/60 backdrop-blur-xs"></div>
        <div class="relative w-full max-w-md bg-white rounded-3xl shadow-2xl p-8 text-center z-10">
            <div class="w-16 h-16 rounded-full bg-emerald-100 text-emerald-600 flex items-center justify-center mx-auto mb-4">
                <i data-lucide="check" class="w-8 h-8"></i>
            </div>
            <span class="text-xs font-bold uppercase tracking-widest text-amber-600">Pesanan Diterima</span>
            <h3 class="font-serif-display text-3xl font-bold text-stone-900 mt-1 mb-2">Terima Kasih!</h3>
            <p class="text-sm text-stone-600 mb-6">
                Pesanan Anda telah tersimpan di database toko kami. Tim Aurelia akan segera menghubungi Anda melalui WhatsApp untuk konfirmasi pengiriman.
            </p>
            <button type="button" onclick="closeSuccessModal()" class="w-full py-3 rounded-full bg-stone-900 hover:bg-stone-800 text-white font-semibold text-sm transition-all">
                Kembali Berbelanja
            </button>
        </div>
    </div>

    <!-- ==================== HYBRID DATABASE ENGINE & APP LOGIC ==================== -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, signInWithCustomToken } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { 
            getFirestore, collection, doc, setDoc, updateDoc, deleteDoc, 
            onSnapshot, getDocs, writeBatch 
        } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // ==================== KONSTANTA & DATA AWAL ====================
        const DEFAULT_FALLBACK_IMG = "https://images.unsplash.com/photo-1515562141207-7a88fb7ce338?auto=format&fit=crop&w=800&q=80";
        const STORAGE_KEYS = {
            PRODUCTS: 'aurelia_boutique_products_v2',
            ORDERS: 'aurelia_boutique_orders_v2'
        };

        const SEED_PRODUCTS = [
            {
                id: "prod_seed_1",
                name: "Kalung Liontin Mutiara Aurelia",
                category: "Kalung",
                price: 345000,
                stock: 14,
                image: "https://images.unsplash.com/photo-1599643478518-a784e5dc4c8f?auto=format&fit=crop&w=800&q=80",
                description: "Kalung rantai emas 18K plated dengan liontin mutiara air tawar alami yang anggun.",
                createdAt: 1700000001000
            },
            {
                id: "prod_seed_2",
                name: "Gelang Rantai Serpentine Klasik",
                category: "Gelang",
                price: 220000,
                stock: 20,
                image: "https://images.unsplash.com/photo-1611591437281-460bfbe1220a?auto=format&fit=crop&w=800&q=80",
                description: "Gelang pipih berkilau anti-karat, cocok digunakan tunggal maupun layering.",
                createdAt: 1700000002000
            },
            {
                id: "prod_seed_3",
                name: "Cincin Solitaire Zirconia Minimalis",
                category: "Cincin",
                price: 185000,
                stock: 18,
                image: "https://images.unsplash.com/photo-1605100804763-247f67b3557e?auto=format&fit=crop&w=800&q=80",
                description: "Cincin elegan berhiaskan batu cubic zirconia grade AAA dengan kilau menyerupai berlian.",
                createdAt: 1700000003000
            },
            {
                id: "prod_seed_4",
                name: "Anting Hoop Emas Lilit Vintage",
                category: "Anting",
                price: 165000,
                stock: 25,
                image: "https://images.unsplash.com/photo-1535632066927-ab7c9ab60908?auto=format&fit=crop&w=800&q=80",
                description: "Anting bulat bertekstur lilit ringan dan hipoalergenik untuk pemakaian sepanjang hari.",
                createdAt: 1700000004000
            },
            {
                id: "prod_seed_5",
                name: "Jam Tangan Petite Mesh Rose Gold",
                category: "Jam Tangan",
                price: 780000,
                stock: 8,
                image: "https://images.unsplash.com/photo-1524592094714-0f0654e20314?auto=format&fit=crop&w=800&q=80",
                description: "Jam tangan analog minimalis dengan tali stainless steel mesh dan kaca tahan gores.",
                createdAt: 1700000005000
            },
            {
                id: "prod_seed_6",
                name: "Kalung Layering Bulan Sabit",
                category: "Kalung",
                price: 275000,
                stock: 12,
                image: "https://images.unsplash.com/photo-1602751584552-8ba73aad10e1?auto=format&fit=crop&w=800&q=80",
                description: "Dua tingkat kalung tipis dengan aksen bulan sabit yang memberikan kesan chic modern.",
                createdAt: 1700000006000
            },
            {
                id: "prod_seed_7",
                name: "Gelang Bangle Kristal Swarovski",
                category: "Gelang",
                price: 410000,
                stock: 9,
                image: "https://images.unsplash.com/photo-1573408301185-9146fe634ad0?auto=format&fit=crop&w=800&q=80",
                description: "Bangle kokoh dengan deretan kristal berkilau dan pengunci tersembunyi yang aman.",
                createdAt: 1700000007000
            },
            {
                id: "prod_seed_8",
                name: "Jam Tangan Kulit Klasik Chronos",
                category: "Jam Tangan",
                price: 890000,
                stock: 6,
                image: "https://images.unsplash.com/photo-1522312346375-d1a52e2b99b3?auto=format&fit=crop&w=800&q=80",
                description: "Jam tangan dial ivory dengan strap kulit asli berwarna cokelat tua yang timeless.",
                createdAt: 1700000008000
            }
        ];

        // ==================== APPLICATION STATE ====================
        let products = [];
        let orders = [];
        let cart = [];
        let selectedCategory = 'Semua';
        let isSavingProduct = false;
        let uploadedImageDataUrl = "";
        let confirmCallback = null;

        // Hybrid Database Engine State
        let cloudReady = false;
        let db = null;
        let auth = null;
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'aurelia-accessories-store';
        const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : null;

        // ==================== UTILITY HELPERS ====================
        window.formatRupiah = (number) => {
            return new Intl.NumberFormat('id-ID', {
                style: 'currency',
                currency: 'IDR',
                minimumFractionDigits: 0
            }).format(Number(number) || 0);
        };

        window.showToast = (message, type = 'success') => {
            const container = document.getElementById('toast-container');
            if (!container) return;

            const styles = {
                success: 'bg-stone-900 text-ivory border-amber-500/40',
                warning: 'bg-amber-900 text-amber-50 border-amber-400/50',
                error: 'bg-red-900 text-red-50 border-red-400/50'
            };
            const icons = {
                success: 'check-circle-2',
                warning: 'alert-circle',
                error: 'x-circle'
            };

            const toast = document.createElement('div');
            toast.className = `pointer-events-auto flex items-center gap-3 px-4 py-3.5 rounded-2xl border shadow-xl text-xs sm:text-sm font-medium transition-all duration-300 translate-y-2 opacity-0 ${styles[type] || styles.success}`;
            toast.innerHTML = `
                <i data-lucide="${icons[type] || 'info'}" class="w-4 h-4 text-amber-400 shrink-0"></i>
                <span class="flex-1 leading-snug">${message}</span>
            `;

            container.appendChild(toast);
            lucide.createIcons();

            requestAnimationFrame(() => {
                toast.classList.remove('translate-y-2', 'opacity-0');
            });

            setTimeout(() => {
                toast.classList.add('opacity-0', 'translate-y-2');
                setTimeout(() => toast.remove(), 300);
            }, 3200);
        };

        function generateId(prefix = 'item') {
            return `${prefix}_${Date.now()}_${Math.random().toString(36).substring(2, 7)}`;
        }

        function isValidImageSource(str) {
            if (!str || typeof str !== 'string') return false;
            const trimmed = str.trim();
            return trimmed.startsWith('http://') || trimmed.startsWith('https://') || trimmed.startsWith('data:image/');
        }

        function updateDatabaseStatusUI(isCloud) {
            const label = isCloud ? "Database Aktif • Cloud Sync" : "Database Aktif • Penyimpanan Lokal";
            const headerText = document.getElementById('header-db-text');
            const adminText = document.getElementById('admin-db-text');
            if (headerText) headerText.textContent = label;
            if (adminText) adminText.textContent = label;
        }

        // ==================== HYBRID DATABASE ENGINE ====================
        function readLocalProducts() {
            try {
                const raw = localStorage.getItem(STORAGE_KEYS.PRODUCTS);
                if (!raw) return null;
                const parsed = JSON.parse(raw);
                return Array.isArray(parsed) ? parsed : null;
            } catch (e) {
                return null;
            }
        }

        function writeLocalProducts(list) {
            try {
                localStorage.setItem(STORAGE_KEYS.PRODUCTS, JSON.stringify(list));
            } catch (e) {
                console.warn("Local storage quota warning:", e);
            }
        }

        function readLocalOrders() {
            try {
                const raw = localStorage.getItem(STORAGE_KEYS.ORDERS);
                if (!raw) return [];
                const parsed = JSON.parse(raw);
                return Array.isArray(parsed) ? parsed : [];
            } catch (e) {
                return [];
            }
        }

        function writeLocalOrders(list) {
            try {
                localStorage.setItem(STORAGE_KEYS.ORDERS, JSON.stringify(list));
            } catch (e) {
                console.warn("Local storage orders warning:", e);
            }
        }

        function syncAndRenderAll() {
            products.sort((a, b) => (b.createdAt || 0) - (a.createdAt || 0));
            orders.sort((a, b) => (b.createdAt || 0) - (a.createdAt || 0));

            writeLocalProducts(products);
            writeLocalOrders(orders);

            const loadingEl = document.getElementById('products-loading');
            if (loadingEl) loadingEl.classList.add('hidden');

            renderProducts();
            renderAdminProducts();
            renderAdminOrders();
            updateAdminStats();
        }

        // Simpan / Update Produk ke Hybrid Engine (Lokal instan + Cloud background)
        async function dbSaveProduct(productObj, isEdit = false) {
            if (isEdit) {
                const idx = products.findIndex(p => p.id === productObj.id);
                if (idx !== -1) {
                    products[idx] = { ...products[idx], ...productObj };
                } else {
                    products.unshift(productObj);
                }
            } else {
                products.unshift(productObj);
            }

            // Simpan ke LocalStorage & render UI secara instan (0ms latency)
            syncAndRenderAll();

            // Sinkronisasi ke Cloud Firestore secara background jika aktif
            if (cloudReady && db && auth?.currentUser) {
                try {
                    const docRef = doc(db, 'artifacts', appId, 'public', 'data', 'products', productObj.id);
                    await setDoc(docRef, productObj, { merge: true });
                } catch (err) {
                    console.warn("Cloud sync write fallback to local:", err.message);
                    cloudReady = false;
                    updateDatabaseStatusUI(false);
                }
            }
        }

        // Hapus Produk dari Hybrid Engine
        async function dbDeleteProduct(productId) {
            products = products.filter(p => p.id !== productId);
            syncAndRenderAll();

            if (cloudReady && db && auth?.currentUser) {
                try {
                    const docRef = doc(db, 'artifacts', appId, 'public', 'data', 'products', productId);
                    await deleteDoc(docRef);
                } catch (err) {
                    console.warn("Cloud delete fallback:", err.message);
                }
            }
        }

        // Simpan Pesanan Baru & Kurangi Stok
        async function dbCreateOrder(orderObj, purchasedItems) {
            orders.unshift(orderObj);

            purchasedItems.forEach(item => {
                const prodIdx = products.findIndex(p => p.id === item.productId);
                if (prodIdx !== -1) {
                    const currentStock = Number(products[prodIdx].stock) || 0;
                    products[prodIdx].stock = Math.max(0, currentStock - (Number(item.qty) || 1));
                }
            });

            syncAndRenderAll();

            if (cloudReady && db && auth?.currentUser) {
                try {
                    const batch = writeBatch(db);
                    const orderRef = doc(db, 'artifacts', appId, 'public', 'data', 'orders', orderObj.id);
                    batch.set(orderRef, orderObj);

                    purchasedItems.forEach(item => {
                        const prod = products.find(p => p.id === item.productId);
                        if (prod) {
                            const prodRef = doc(db, 'artifacts', appId, 'public', 'data', 'products', prod.id);
                            batch.set(prodRef, { stock: prod.stock }, { merge: true });
                        }
                    });
                    await batch.commit();
                } catch (err) {
                    console.warn("Cloud order sync fallback:", err.message);
                }
            }
        }

        // Update Status Pesanan
        async function dbUpdateOrderStatus(orderId, newStatus) {
            const idx = orders.findIndex(o => o.id === orderId);
            if (idx !== -1) {
                orders[idx].status = newStatus;
            }
            syncAndRenderAll();

            if (cloudReady && db && auth?.currentUser) {
                try {
                    const orderRef = doc(db, 'artifacts', appId, 'public', 'data', 'orders', orderId);
                    await updateDoc(orderRef, { status: newStatus });
                } catch (err) {
                    console.warn("Cloud order status fallback:", err.message);
                }
            }
        }

        // Hapus Pesanan
        async function dbDeleteOrder(orderId) {
            orders = orders.filter(o => o.id !== orderId);
            syncAndRenderAll();

            if (cloudReady && db && auth?.currentUser) {
                try {
                    const orderRef = doc(db, 'artifacts', appId, 'public', 'data', 'orders', orderId);
                    await deleteDoc(orderRef);
                } catch (err) {
                    console.warn("Cloud delete order fallback:", err.message);
                }
            }
        }

        // Inisialisasi Hybrid Database (Lokal langsung siap + coba Cloud dengan timeout 2.5 detik)
        async function initHybridDatabase() {
            lucide.createIcons();

            // 1. Muat dari LocalStorage segera agar tidak ada layar tunggu
            const localProds = readLocalProducts();
            if (localProds && localProds.length > 0) {
                products = localProds;
            } else {
                products = SEED_PRODUCTS.map(item => ({ ...item }));
                writeLocalProducts(products);
            }
            orders = readLocalOrders();
            updateDatabaseStatusUI(false);
            syncAndRenderAll();

            // 2. Coba sambungkan Firebase dengan timeout 2.5 detik
            if (!firebaseConfig) return;

            try {
                const app = initializeApp(firebaseConfig);
                db = getFirestore(app);
                auth = getAuth(app);

                const authPromise = (typeof __initial_auth_token !== 'undefined' && __initial_auth_token)
                    ? signInWithCustomToken(auth, __initial_auth_token)
                    : signInAnonymously(auth);

                const timeoutPromise = new Promise((_, reject) =>
                    setTimeout(() => reject(new Error("Firebase connection timeout")), 2500)
                );

                await Promise.race([authPromise, timeoutPromise]);

                // Uji izin baca/tulis cepat ke Firestore
                const productsCol = collection(db, 'artifacts', appId, 'public', 'data', 'products');
                const testSnap = await Promise.race([
                    getDocs(productsCol),
                    new Promise((_, reject) => setTimeout(() => reject(new Error("Firestore read timeout")), 2500))
                ]);

                // Jika cloud kosong, unggah data lokal saat ini ke cloud
                if (testSnap.empty && products.length > 0) {
                    const batch = writeBatch(db);
                    products.forEach(p => {
                        const ref = doc(db, 'artifacts', appId, 'public', 'data', 'products', p.id);
                        batch.set(ref, p);
                    });
                    await batch.commit();
                }

                cloudReady = true;
                updateDatabaseStatusUI(true);

                // Pasang listener real-time untuk sinkronisasi dua arah
                onSnapshot(productsCol, (snapshot) => {
                    if (!snapshot.empty) {
                        products = snapshot.docs.map(d => ({ id: d.id, ...d.data() }));
                        syncAndRenderAll();
                    }
                }, (err) => {
                    console.warn("Firestore snapshot listener disabled:", err.message);
                    cloudReady = false;
                    updateDatabaseStatusUI(false);
                });

                const ordersCol = collection(db, 'artifacts', appId, 'public', 'data', 'orders');
                onSnapshot(ordersCol, (snapshot) => {
                    orders = snapshot.docs.map(d => ({ id: d.id, ...d.data() }));
                    syncAndRenderAll();
                }, () => {});

            } catch (err) {
                // Seamless fallback ke penyimpanan lokal tanpa pesan error mengganggu
                console.info("Menggunakan penyimpanan persisten lokal:", err.message);
                cloudReady = false;
                updateDatabaseStatusUI(false);
            }
        }

        // ==================== STOREFRONT RENDERING ====================
        window.setCategory = (category) => {
            selectedCategory = category;
            document.querySelectorAll('.cat-btn').forEach(btn => {
                if (btn.dataset.cat === category) {
                    btn.className = "cat-btn active px-4 py-2 rounded-full text-sm font-medium bg-stone-900 text-white transition-all whitespace-nowrap shadow-xs";
                } else {
                    btn.className = "cat-btn px-4 py-2 rounded-full text-sm font-medium bg-sand text-stone-700 hover:bg-stone-200 transition-all whitespace-nowrap";
                }
            });
            renderProducts();
        };

        window.resetFilters = () => {
            document.getElementById('search-input').value = '';
            setCategory('Semua');
        };

        window.renderProducts = () => {
            const grid = document.getElementById('product-grid');
            const emptyState = document.getElementById('products-empty');
            const searchQuery = document.getElementById('search-input').value.toLowerCase().trim();

            const filtered = products.filter(p => {
                const matchCat = selectedCategory === 'Semua' || p.category === selectedCategory;
                const matchSearch = (p.name || '').toLowerCase().includes(searchQuery) || 
                                    (p.description || '').toLowerCase().includes(searchQuery);
                return matchCat && matchSearch;
            });

            if (filtered.length === 0) {
                grid.innerHTML = '';
                emptyState.classList.remove('hidden');
                return;
            }

            emptyState.classList.add('hidden');
            grid.innerHTML = filtered.map(prod => {
                const stockNum = Number(prod.stock) || 0;
                const isOutOfStock = stockNum <= 0;
                const fallbackImg = `https://placehold.co/600x600/F2EDE4/1C1917?text=${encodeURIComponent(prod.name || 'Aksesoris')}`;
                const imgSrc = isValidImageSource(prod.image) ? prod.image : fallbackImg;
                
                return `
                <div class="product-card bg-white rounded-3xl border border-stone-200/70 overflow-hidden shadow-xs hover:shadow-xl transition-all duration-300 flex flex-col">
                    <div class="relative aspect-square bg-sand overflow-hidden">
                        <img src="${imgSrc}" 
                             alt="${prod.name}" 
                             onerror="this.src='${fallbackImg}'"
                             class="product-img w-full h-full object-cover transition-transform duration-500">
                        <span class="absolute top-3 left-3 px-3 py-1 rounded-full bg-white/90 backdrop-blur-xs text-stone-800 text-xs font-semibold shadow-xs">
                            ${prod.category}
                        </span>
                        ${isOutOfStock ? `
                            <div class="absolute inset-0 bg-stone-950/60 flex items-center justify-center">
                                <span class="px-4 py-1.5 rounded-full bg-red-600 text-white text-xs font-bold uppercase tracking-wider">Stok Habis</span>
                            </div>
                        ` : `
                            <span class="absolute bottom-3 right-3 px-2.5 py-1 rounded-full bg-stone-900/75 text-ivory text-[11px] font-medium">
                                Stok: ${stockNum}
                            </span>
                        `}
                    </div>
                    
                    <div class="p-5 flex-1 flex flex-col">
                        <h3 class="font-serif-display text-xl font-bold text-stone-900 leading-snug mb-1">${prod.name}</h3>
                        <p class="text-xs text-stone-500 line-clamp-2 mb-4 flex-1">${prod.description || 'Aksesoris eksklusif koleksi Aurelia Boutique.'}</p>
                        
                        <div class="pt-3 border-t border-stone-100 flex items-center justify-between gap-2">
                            <div>
                                <span class="text-[10px] uppercase tracking-wider text-stone-400 block">Harga</span>
                                <span class="font-bold text-base text-amber-700">${formatRupiah(prod.price)}</span>
                            </div>
                            <button type="button" onclick="addToCart('${prod.id}')" 
                                    ${isOutOfStock ? 'disabled' : ''}
                                    class="px-4 py-2.5 rounded-full ${isOutOfStock ? 'bg-stone-200 text-stone-400 cursor-not-allowed' : 'bg-stone-900 hover:bg-amber-600 text-white active:scale-95'} text-xs font-semibold transition-all flex items-center gap-1.5 shadow-xs">
                                <i data-lucide="shopping-bag" class="w-3.5 h-3.5"></i>
                                <span>${isOutOfStock ? 'Habis' : '+ Keranjang'}</span>
                            </button>
                        </div>
                    </div>
                </div>
                `;
            }).join('');

            lucide.createIcons();
        };

        // ==================== SHOPPING CART & CHECKOUT ====================
        window.toggleCart = (open) => {
            const drawer = document.getElementById('cart-drawer');
            if (open) {
                drawer.classList.remove('hidden');
                showCheckoutStep(false);
                renderCart();
            } else {
                drawer.classList.add('hidden');
            }
        };

        window.showCheckoutStep = (showForm) => {
            const itemsStep = document.getElementById('cart-items-step');
            const formStep = document.getElementById('checkout-form-step');
            if (showForm) {
                itemsStep.classList.add('hidden');
                formStep.classList.remove('hidden');
                const totalQty = cart.reduce((sum, item) => sum + item.qty, 0);
                const totalPrice = cart.reduce((sum, item) => sum + (item.price * item.qty), 0);
                document.getElementById('checkout-item-count').textContent = `${totalQty} produk`;
                document.getElementById('checkout-total-display').textContent = formatRupiah(totalPrice);
            } else {
                formStep.classList.add('hidden');
                itemsStep.classList.remove('hidden');
            }
        };

        window.addToCart = (productId) => {
            const product = products.find(p => p.id === productId);
            if (!product || (Number(product.stock) || 0) <= 0) return;

            const existing = cart.find(item => item.id === productId);
            if (existing) {
                if (existing.qty >= product.stock) {
                    showToast(`Maksimal stok tersedia hanya ${product.stock} item.`, "warning");
                    return;
                }
                existing.qty += 1;
            } else {
                cart.push({
                    id: product.id,
                    name: product.name,
                    price: Number(product.price) || 0,
                    image: product.image,
                    qty: 1
                });
            }

            updateCartBadge();
            showToast(`"${product.name}" ditambahkan ke keranjang.`);
        };

        window.updateCartQty = (productId, delta) => {
            const index = cart.findIndex(item => item.id === productId);
            if (index === -1) return;

            const product = products.find(p => p.id === productId);
            const maxStock = product ? (Number(product.stock) || 0) : 99;
            const newQty = cart[index].qty + delta;

            if (newQty <= 0) {
                cart.splice(index, 1);
            } else if (newQty > maxStock) {
                showToast(`Stok tersisa hanya ${maxStock} item.`, "warning");
                return;
            } else {
                cart[index].qty = newQty;
            }

            updateCartBadge();
            renderCart();
        };

        window.removeFromCart = (productId) => {
            cart = cart.filter(item => item.id !== productId);
            updateCartBadge();
            renderCart();
        };

        function updateCartBadge() {
            const totalItems = cart.reduce((sum, item) => sum + item.qty, 0);
            document.getElementById('cart-badge').textContent = totalItems;
        }

        function renderCart() {
            const container = document.getElementById('cart-items-container');
            const emptyMsg = document.getElementById('empty-cart-msg');
            const footer = document.getElementById('cart-footer');

            if (cart.length === 0) {
                container.classList.add('hidden');
                footer.classList.add('hidden');
                emptyMsg.classList.remove('hidden');
                return;
            }

            container.classList.remove('hidden');
            footer.classList.remove('hidden');
            emptyMsg.classList.add('hidden');

            let total = 0;
            container.innerHTML = cart.map(item => {
                const subtotal = item.price * item.qty;
                total += subtotal;
                const fallbackImg = `https://placehold.co/150x150/F2EDE4/1C1917?text=Item`;
                const imgSrc = isValidImageSource(item.image) ? item.image : fallbackImg;

                return `
                <div class="flex gap-3.5 bg-white p-3.5 rounded-2xl border border-stone-200/80 shadow-2xs">
                    <img src="${imgSrc}" onerror="this.src='${fallbackImg}'" alt="${item.name}" class="w-16 h-16 rounded-xl object-cover bg-sand shrink-0">
                    <div class="flex-1 min-w-0">
                        <div class="flex items-start justify-between gap-2">
                            <h4 class="font-semibold text-sm text-stone-900 truncate">${item.name}</h4>
                            <button type="button" onclick="removeFromCart('${item.id}')" class="text-stone-400 hover:text-red-500 transition-colors p-0.5">
                                <i data-lucide="trash-2" class="w-4 h-4"></i>
                            </button>
                        </div>
                        <p class="text-xs font-bold text-amber-700 mt-0.5">${formatRupiah(item.price)}</p>
                        
                        <div class="flex items-center justify-between mt-2.5">
                            <div class="flex items-center bg-sand rounded-full p-0.5">
                                <button type="button" onclick="updateCartQty('${item.id}', -1)" class="w-6 h-6 rounded-full bg-white text-stone-700 flex items-center justify-center hover:bg-stone-200 text-xs font-bold">-</button>
                                <span class="px-3 text-xs font-semibold">${item.qty}</span>
                                <button type="button" onclick="updateCartQty('${item.id}', 1)" class="w-6 h-6 rounded-full bg-white text-stone-700 flex items-center justify-center hover:bg-stone-200 text-xs font-bold">+</button>
                            </div>
                            <span class="text-xs font-semibold text-stone-600">${formatRupiah(subtotal)}</span>
                        </div>
                    </div>
                </div>
                `;
            }).join('');

            document.getElementById('cart-subtotal').textContent = formatRupiah(total);
            document.getElementById('cart-total').textContent = formatRupiah(total);
            lucide.createIcons();
        }

        // Submit Checkout Form
        document.getElementById('checkout-form').addEventListener('submit', async (e) => {
            e.preventDefault();
            if (cart.length === 0) return;

            const customerName = document.getElementById('cust-name').value.trim();
            const customerPhone = document.getElementById('cust-phone').value.trim();
            const customerAddress = document.getElementById('cust-address').value.trim();
            const paymentMethod = document.getElementById('cust-payment').value;

            if (!customerName) {
                showToast("Mohon masukkan nama lengkap penerima.", "warning");
                return;
            }
            if (!customerPhone) {
                showToast("Mohon masukkan nomor WhatsApp yang aktif.", "warning");
                return;
            }
            if (!customerAddress) {
                showToast("Mohon masukkan alamat lengkap pengiriman.", "warning");
                return;
            }

            const totalAmount = cart.reduce((sum, item) => sum + (item.price * item.qty), 0);
            const purchasedItems = cart.map(c => ({
                productId: c.id,
                name: c.name,
                price: Number(c.price) || 0,
                qty: Number(c.qty) || 1
            }));

            const newOrder = {
                id: generateId('ord'),
                customerName,
                customerPhone,
                customerAddress,
                paymentMethod,
                items: purchasedItems,
                totalAmount,
                status: 'Menunggu',
                createdAt: Date.now()
            };

            await dbCreateOrder(newOrder, purchasedItems);

            cart = [];
            updateCartBadge();
            document.getElementById('checkout-form').reset();
            toggleCart(false);
            document.getElementById('order-success-modal').classList.remove('hidden');
        });

        window.closeSuccessModal = () => {
            document.getElementById('order-success-modal').classList.add('hidden');
        };

        // ==================== ADMIN PANEL & VIEW NAVIGATION ====================
        window.switchView = (view) => {
            const storeView = document.getElementById('store-view');
            const adminView = document.getElementById('admin-view');
            const storeBtn = document.getElementById('nav-store-btn');
            const adminBtn = document.getElementById('nav-admin-btn');

            if (view === 'admin') {
                storeView.classList.add('hidden');
                adminView.classList.remove('hidden');
                adminBtn.className = "px-3 sm:px-4 py-1.5 rounded-full bg-stone-900 text-white shadow-sm transition-all flex items-center gap-1.5";
                storeBtn.className = "px-3 sm:px-4 py-1.5 rounded-full text-stone-600 hover:text-stone-900 transition-all flex items-center gap-1.5";
            } else {
                adminView.classList.add('hidden');
                storeView.classList.remove('hidden');
                storeBtn.className = "px-3 sm:px-4 py-1.5 rounded-full bg-stone-900 text-white shadow-sm transition-all flex items-center gap-1.5";
                adminBtn.className = "px-3 sm:px-4 py-1.5 rounded-full text-stone-600 hover:text-stone-900 transition-all flex items-center gap-1.5";
            }
        };

        window.switchAdminTab = (tab) => {
            const prodSec = document.getElementById('admin-products-section');
            const ordSec = document.getElementById('admin-orders-section');
            const prodTab = document.getElementById('tab-admin-products');
            const ordTab = document.getElementById('tab-admin-orders');

            if (tab === 'orders') {
                prodSec.classList.add('hidden');
                ordSec.classList.remove('hidden');
                ordTab.className = "px-5 py-3 text-sm font-semibold border-b-2 border-stone-900 text-stone-900 transition-all flex items-center gap-2 whitespace-nowrap";
                prodTab.className = "px-5 py-3 text-sm font-medium border-b-2 border-transparent text-stone-500 hover:text-stone-900 transition-all flex items-center gap-2 whitespace-nowrap";
            } else {
                ordSec.classList.add('hidden');
                prodSec.classList.remove('hidden');
                prodTab.className = "px-5 py-3 text-sm font-semibold border-b-2 border-stone-900 text-stone-900 transition-all flex items-center gap-2 whitespace-nowrap";
                ordTab.className = "px-5 py-3 text-sm font-medium border-b-2 border-transparent text-stone-500 hover:text-stone-900 transition-all flex items-center gap-2 whitespace-nowrap";
            }
        };

        function updateAdminStats() {
            document.getElementById('stat-total-products').textContent = products.length;
            const totalStock = products.reduce((sum, p) => sum + (Number(p.stock) || 0), 0);
            document.getElementById('stat-total-stock').textContent = `${totalStock} pcs`;
            document.getElementById('stat-total-orders').textContent = orders.length;
            document.getElementById('admin-order-badge').textContent = orders.length;

            const totalRev = orders.reduce((sum, o) => sum + (Number(o.totalAmount) || 0), 0);
            document.getElementById('stat-revenue').textContent = formatRupiah(totalRev);
        }

        function renderAdminProducts() {
            const tbody = document.getElementById('admin-products-tbody');
            if (products.length === 0) {
                tbody.innerHTML = `<tr><td colspan="5" class="py-12 text-center text-stone-400">Belum ada produk di database. Klik "Tambah Produk" untuk mulai mengisi katalog.</td></tr>`;
                return;
            }

            tbody.innerHTML = products.map(prod => {
                const fallbackImg = `https://placehold.co/100x100/F2EDE4/1C1917?text=Img`;
                const imgSrc = isValidImageSource(prod.image) ? prod.image : fallbackImg;
                const stockNum = Number(prod.stock) || 0;

                return `
                <tr class="hover:bg-stone-50/80 transition-colors">
                    <td class="py-3.5 px-6">
                        <div class="flex items-center gap-3">
                            <img src="${imgSrc}" onerror="this.src='${fallbackImg}'" class="w-11 h-11 rounded-xl object-cover bg-sand shrink-0">
                            <div class="min-w-0">
                                <p class="font-semibold text-stone-900 truncate max-w-[200px] sm:max-w-xs">${prod.name}</p>
                                <p class="text-xs text-stone-400 line-clamp-1 max-w-xs">${prod.description || '-'}</p>
                            </div>
                        </div>
                    </td>
                    <td class="py-3.5 px-6">
                        <span class="px-2.5 py-1 rounded-full bg-sand text-stone-700 text-xs font-medium whitespace-nowrap">${prod.category}</span>
                    </td>
                    <td class="py-3.5 px-6 font-semibold text-stone-900 whitespace-nowrap">${formatRupiah(prod.price)}</td>
                    <td class="py-3.5 px-6 whitespace-nowrap">
                        <span class="px-2.5 py-1 rounded-full text-xs font-bold ${stockNum > 5 ? 'bg-emerald-100 text-emerald-800' : (stockNum > 0 ? 'bg-amber-100 text-amber-800' : 'bg-red-100 text-red-700')}">
                            ${stockNum} pcs
                        </span>
                    </td>
                    <td class="py-3.5 px-6 text-right whitespace-nowrap">
                        <div class="inline-flex items-center gap-1.5">
                            <button type="button" onclick="openProductModal('${prod.id}')" class="p-2 rounded-xl bg-stone-100 hover:bg-amber-100 text-stone-700 hover:text-amber-800 transition-colors" title="Edit Produk">
                                <i data-lucide="pencil" class="w-4 h-4"></i>
                            </button>
                            <button type="button" onclick="confirmDeleteProduct('${prod.id}', '${(prod.name || '').replace(/'/g, "\\'")}')" class="p-2 rounded-xl bg-stone-100 hover:bg-red-100 text-stone-700 hover:text-red-600 transition-colors" title="Hapus Produk">
                                <i data-lucide="trash-2" class="w-4 h-4"></i>
                            </button>
                        </div>
                    </td>
                </tr>
                `;
            }).join('');

            lucide.createIcons();
        }

        function renderAdminOrders() {
            const list = document.getElementById('admin-orders-list');
            if (orders.length === 0) {
                list.innerHTML = `
                    <div class="col-span-full bg-white rounded-3xl border border-stone-200/80 p-12 text-center">
                        <i data-lucide="inbox" class="w-10 h-10 text-stone-300 mx-auto mb-2"></i>
                        <p class="text-stone-500 text-sm">Belum ada pesanan masuk dari pelanggan.</p>
                    </div>
                `;
                lucide.createIcons();
                return;
            }

            list.innerHTML = orders.map(order => {
                const dateStr = order.createdAt ? new Date(order.createdAt).toLocaleString('id-ID', {
                    day: 'numeric', month: 'short', year: 'numeric', hour: '2-digit', minute: '2-digit'
                }) : '-';

                const statusColors = {
                    'Menunggu': 'bg-amber-100 text-amber-800 border-amber-200',
                    'Diproses': 'bg-blue-100 text-blue-800 border-blue-200',
                    'Dikirim': 'bg-purple-100 text-purple-800 border-purple-200',
                    'Selesai': 'bg-emerald-100 text-emerald-800 border-emerald-200'
                };
                const badgeClass = statusColors[order.status] || 'bg-stone-100 text-stone-800';

                return `
                <div class="bg-white rounded-3xl border border-stone-200/80 p-6 shadow-xs flex flex-col justify-between">
                    <div>
                        <div class="flex items-start justify-between gap-2 pb-4 border-b border-stone-100">
                            <div>
                                <span class="text-[11px] text-stone-400 font-mono">#${order.id.slice(-6).toUpperCase()} • ${dateStr}</span>
                                <h4 class="font-bold text-base text-stone-900 mt-0.5">${order.customerName}</h4>
                                <p class="text-xs text-stone-500 flex items-center gap-1 mt-0.5">
                                    <i data-lucide="phone" class="w-3 h-3"></i> ${order.customerPhone}
                                </p>
                            </div>
                            <div class="flex items-center gap-1.5">
                                <span class="px-3 py-1 rounded-full text-xs font-bold border ${badgeClass}">
                                    ${order.status}
                                </span>
                                <button type="button" onclick="confirmDeleteOrder('${order.id}')" class="p-1.5 rounded-lg text-stone-400 hover:text-red-600 hover:bg-red-50 transition-colors" title="Hapus Pesanan">
                                    <i data-lucide="trash-2" class="w-4 h-4"></i>
                                </button>
                            </div>
                        </div>

                        <div class="py-3 text-xs text-stone-600 space-y-1 border-b border-stone-100">
                            <p><strong class="text-stone-800">Alamat:</strong> ${order.customerAddress}</p>
                            <p><strong class="text-stone-800">Pembayaran:</strong> ${order.paymentMethod}</p>
                        </div>

                        <div class="py-3 space-y-1.5">
                            <p class="text-[11px] font-semibold uppercase tracking-wider text-stone-400">Rincian Aksesoris:</p>
                            ${(order.items || []).map(item => `
                                <div class="flex justify-between text-xs text-stone-700">
                                    <span>${item.qty}x ${item.name}</span>
                                    <span class="font-medium">${formatRupiah(item.price * item.qty)}</span>
                                </div>
                            `).join('')}
                        </div>
                    </div>

                    <div class="pt-4 border-t border-stone-100 flex flex-wrap items-center justify-between gap-3">
                        <div>
                            <span class="text-[10px] uppercase text-stone-400 block">Total Pesanan</span>
                            <span class="font-bold text-base text-amber-700">${formatRupiah(order.totalAmount)}</span>
                        </div>

                        <div class="flex items-center gap-2">
                            <label class="text-xs text-stone-500">Status:</label>
                            <select onchange="handleOrderStatusChange('${order.id}', this.value)" 
                                    class="px-3 py-1.5 rounded-xl bg-sand border border-stone-200 text-xs font-semibold text-stone-800 focus:outline-none focus:border-amber-500">
                                <option value="Menunggu" ${order.status === 'Menunggu' ? 'selected' : ''}>Menunggu</option>
                                <option value="Diproses" ${order.status === 'Diproses' ? 'selected' : ''}>Diproses</option>
                                <option value="Dikirim" ${order.status === 'Dikirim' ? 'selected' : ''}>Dikirim</option>
                                <option value="Selesai" ${order.status === 'Selesai' ? 'selected' : ''}>Selesai</option>
                            </select>
                        </div>
                    </div>
                </div>
                `;
            }).join('');

            lucide.createIcons();
        }

        // ==================== UPLOAD GAMBAR TERKOMPRESI VIA CANVAS ====================
        document.getElementById('prod-file-input').addEventListener('change', (e) => {
            const file = e.target.files?.[0];
            if (!file) return;

            if (!file.type.startsWith('image/')) {
                showToast("File yang dipilih harus berupa gambar (JPG/PNG/WEBP).", "warning");
                return;
            }

            const reader = new FileReader();
            reader.onload = (event) => {
                const img = new Image();
                img.onload = () => {
                    // Kompresi gambar agar maksimal lebar/tinggi 600px (hemat memori LocalStorage & Firestore)
                    const maxDim = 600;
                    let width = img.width;
                    let height = img.height;

                    if (width > maxDim || height > maxDim) {
                        if (width > height) {
                            height = Math.round((height * maxDim) / width);
                            width = maxDim;
                        } else {
                            width = Math.round((width * maxDim) / height);
                            height = maxDim;
                        }
                    }

                    const canvas = document.createElement('canvas');
                    canvas.width = width;
                    canvas.height = height;
                    const ctx = canvas.getContext('2d');
                    ctx.drawImage(img, 0, 0, width, height);

                    const compressedDataUrl = canvas.toDataURL('image/jpeg', 0.78);
                    uploadedImageDataUrl = compressedDataUrl;
                    document.getElementById('prod-img-preview').src = compressedDataUrl;
                    document.getElementById('prod-image').value = '';
                    showToast("Foto produk berhasil dimuat & dikompresi.");
                };
                img.src = event.target.result;
            };
            reader.readAsDataURL(file);
        });

        // Live preview saat mengetik URL gambar
        document.getElementById('prod-image').addEventListener('input', (e) => {
            const val = e.target.value.trim();
            if (isValidImageSource(val)) {
                uploadedImageDataUrl = "";
                document.getElementById('prod-img-preview').src = val;
            } else if (!uploadedImageDataUrl) {
                document.getElementById('prod-img-preview').src = DEFAULT_FALLBACK_IMG;
            }
        });

        // ==================== MODAL TAMBAH & EDIT PRODUK ====================
        window.openProductModal = (productId = null) => {
            const modal = document.getElementById('product-modal');
            const title = document.getElementById('product-modal-title');
            const form = document.getElementById('product-form');
            const preview = document.getElementById('prod-img-preview');

            form.reset();
            uploadedImageDataUrl = "";
            isSavingProduct = false;

            if (productId) {
                const prod = products.find(p => p.id === productId);
                if (!prod) return;
                title.textContent = "Edit Produk Aksesoris";
                document.getElementById('prod-id').value = prod.id;
                document.getElementById('prod-name').value = prod.name || '';
                document.getElementById('prod-category').value = prod.category || 'Kalung';
                document.getElementById('prod-price').value = prod.price ?? '';
                document.getElementById('prod-stock').value = prod.stock ?? '';
                document.getElementById('prod-desc').value = prod.description || '';

                if (prod.image && prod.image.startsWith('data:image/')) {
                    uploadedImageDataUrl = prod.image;
                    document.getElementById('prod-image').value = '';
                    preview.src = prod.image;
                } else {
                    document.getElementById('prod-image').value = prod.image || '';
                    preview.src = isValidImageSource(prod.image) ? prod.image : DEFAULT_FALLBACK_IMG;
                }
            } else {
                title.textContent = "Tambah Produk Baru";
                document.getElementById('prod-id').value = '';
                document.getElementById('prod-category').value = 'Kalung';
                preview.src = DEFAULT_FALLBACK_IMG;
            }

            modal.classList.remove('hidden');
            lucide.createIcons();
        };

        window.closeProductModal = () => {
            document.getElementById('product-modal').classList.add('hidden');
            isSavingProduct = false;
        };

        // Handler Simpan Produk (Submit Form & Klik Tombol)
        async function processSaveProduct(e) {
            if (e) e.preventDefault();
            if (isSavingProduct) return;

            const id = document.getElementById('prod-id').value.trim();
            const name = document.getElementById('prod-name').value.trim();
            const category = document.getElementById('prod-category').value || 'Kalung';
            const rawPrice = document.getElementById('prod-price').value.trim();
            const rawStock = document.getElementById('prod-stock').value.trim();
            const rawUrl = document.getElementById('prod-image').value.trim();
            const description = document.getElementById('prod-desc').value.trim();

            // Validasi manual yang ramah pengguna
            if (!name) {
                showToast("Nama aksesoris wajib diisi.", "warning");
                document.getElementById('prod-name').focus();
                return;
            }

            const parsedPrice = Number(rawPrice);
            if (!rawPrice || isNaN(parsedPrice) || parsedPrice < 0) {
                showToast("Masukkan harga produk dalam angka yang valid (contoh: 250000).", "warning");
                document.getElementById('prod-price').focus();
                return;
            }

            const parsedStock = rawStock === '' ? 10 : Number(rawStock);
            if (isNaN(parsedStock) || parsedStock < 0) {
                showToast("Masukkan jumlah stok barang yang valid.", "warning");
                document.getElementById('prod-stock').focus();
                return;
            }

            isSavingProduct = true;

            // Tentukan sumber gambar final
            let finalImage = DEFAULT_FALLBACK_IMG;
            if (uploadedImageDataUrl) {
                finalImage = uploadedImageDataUrl;
            } else if (isValidImageSource(rawUrl)) {
                finalImage = rawUrl;
            }

            const isEdit = Boolean(id);
            const productPayload = {
                id: isEdit ? id : generateId('prod'),
                name,
                category,
                price: Math.round(parsedPrice),
                stock: Math.floor(parsedStock),
                image: finalImage,
                description: description || "Aksesoris eksklusif koleksi Aurelia Boutique.",
                updatedAt: Date.now()
            };

            if (!isEdit) {
                productPayload.createdAt = Date.now();
            }

            try {
                await dbSaveProduct(productPayload, isEdit);
                closeProductModal();
                showToast(isEdit ? `Perubahan pada "${name}" berhasil disimpan!` : `Produk "${name}" berhasil ditambahkan!`);
            } catch (err) {
                console.error("Save error:", err);
                showToast("Terjadi kendala saat menyimpan produk.", "error");
            } finally {
                isSavingProduct = false;
            }
        }

        document.getElementById('product-form').addEventListener('submit', processSaveProduct);

        // ==================== CUSTOM CONFIRMATION MODAL (HAPUS / RESET) ====================
        function openConfirmModal({ title, text, buttonText, isDanger = true, onConfirm }) {
            document.getElementById('confirm-modal-title').textContent = title;
            document.getElementById('confirm-modal-text').textContent = text;
            
            const actionBtn = document.getElementById('confirm-modal-action-btn');
            const iconBox = document.getElementById('confirm-modal-icon');
            actionBtn.textContent = buttonText;

            if (isDanger) {
                actionBtn.className = "flex-1 py-2.5 rounded-full bg-red-600 hover:bg-red-700 text-white text-sm font-semibold transition-colors";
                iconBox.className = "w-12 h-12 rounded-full bg-red-100 text-red-600 flex items-center justify-center mx-auto mb-4";
            } else {
                actionBtn.className = "flex-1 py-2.5 rounded-full bg-amber-600 hover:bg-amber-700 text-white text-sm font-semibold transition-colors";
                iconBox.className = "w-12 h-12 rounded-full bg-amber-100 text-amber-600 flex items-center justify-center mx-auto mb-4";
            }

            confirmCallback = onConfirm;
            document.getElementById('confirm-modal').classList.remove('hidden');
            lucide.createIcons();
        }

        window.closeConfirmModal = () => {
            confirmCallback = null;
            document.getElementById('confirm-modal').classList.add('hidden');
        };

        document.getElementById('confirm-modal-action-btn').addEventListener('click', async () => {
            const fn = confirmCallback;
            closeConfirmModal();
            if (typeof fn === 'function') {
                await fn();
            }
        });

        window.confirmDeleteProduct = (id, name) => {
            openConfirmModal({
                title: "Hapus Produk?",
                text: `Apakah Anda yakin ingin menghapus "${name}" dari katalog toko?`,
                buttonText: "Ya, Hapus",
                isDanger: true,
                onConfirm: async () => {
                    await dbDeleteProduct(id);
                    showToast(`Produk "${name}" telah dihapus.`);
                }
            });
        };

        window.confirmDeleteOrder = (orderId) => {
            openConfirmModal({
                title: "Hapus Pesanan?",
                text: "Riwayat pesanan pelanggan ini akan dihapus secara permanen.",
                buttonText: "Ya, Hapus",
                isDanger: true,
                onConfirm: async () => {
                    await dbDeleteOrder(orderId);
                    showToast("Pesanan berhasil dihapus.");
                }
            });
        };

        window.confirmResetDatabase = () => {
            openConfirmModal({
                title: "Reset Data Awal?",
                text: "Katalog akan dikembalikan ke 8 produk aksesoris bawaan Aurelia Boutique.",
                buttonText: "Ya, Reset Data",
                isDanger: false,
                onConfirm: async () => {
                    products = SEED_PRODUCTS.map(item => ({ ...item }));
                    syncAndRenderAll();
                    if (cloudReady && db && auth?.currentUser) {
                        try {
                            const batch = writeBatch(db);
                            products.forEach(p => {
                                const ref = doc(db, 'artifacts', appId, 'public', 'data', 'products', p.id);
                                batch.set(ref, p);
                            });
                            await batch.commit();
                        } catch (e) {}
                    }
                    showToast("Data katalog berhasil direset ke koleksi awal.");
                }
            });
        };

        window.handleOrderStatusChange = async (orderId, newStatus) => {
            await dbUpdateOrderStatus(orderId, newStatus);
            showToast(`Status pesanan diubah menjadi "${newStatus}".`);
        };

        // Start App
        window.addEventListener('DOMContentLoaded', initHybridDatabase);
    </script>
</body>
</html>

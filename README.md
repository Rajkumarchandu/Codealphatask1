# Codealphatask1
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>E-Commerce Platform</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            margin: 0;
            padding: 0;
        }
        .container {
            max-width: 1000px;
            margin: 20px auto;
            padding: 20px;
            background: white;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        header {
            text-align: center;
            margin-bottom: 20px;
        }
        nav ul {
            list-style-type: none;
            padding: 0;
            display: flex;
            justify-content: center;
        }
        nav ul li {
            margin: 0 10px;
        }
        nav ul li a {
            text-decoration: none;
            color: #333;
        }
        main {
            padding: 20px;
        }
        .product, .cart-item, .order-item {
            padding: 10px;
            margin-bottom: 20px;
            background: #f9f9f9;
            border-radius: 4px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .product img, .cart-item img, .order-item img {
            width: 100px;
            height: 100px;
        }
        .buttons {
            display: flex;
            gap: 10px;
        }
        .buttons button {
            padding: 5px 10px;
            font-size: 14px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }
        .add-to-cart {
            background: #28a745;
            color: white;
        }
        .add-to-cart:hover {
            background: #218838;
        }
        .remove {
            background: #dc3545;
            color: white;
        }
        .remove:hover {
            background: #c82333;
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>E-Commerce Platform</h1>
            <nav>
                <ul>
                    <li><a href="#" id="home">Home</a></li>
                    <li><a href="#" id="cart">Cart</a></li>
                    <li><a href="#" id="orders">Orders</a></li>
                    <li><a href="#" id="account">Account</a></li>
                </ul>
            </nav>
        </header>
        <main id="main-content">
            <!-- Main content will be injected here -->
        </main>
    </div>
    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const homeLink = document.getElementById('home');
            const cartLink = document.getElementById('cart');
            const ordersLink = document.getElementById('orders');
            const accountLink = document.getElementById('account');
            const mainContent = document.getElementById('main-content');

            const products = [
                { id: 1, name: 'Product 1', price: 100, img: 'https://via.placeholder.com/100' },
                { id: 2, name: 'Product 2', price: 200, img: 'https://via.placeholder.com/100' },
                { id: 3, name: 'Product 3', price: 300, img: 'https://via.placeholder.com/100' },
            ];
            let cart = [];
            let orders = [];
            let user = null;

            homeLink.addEventListener('click', loadHome);
            cartLink.addEventListener('click', loadCart);
            ordersLink.addEventListener('click', loadOrders);
            accountLink.addEventListener('click', loadAccount);

            loadHome();

            function loadHome() {
                let content = `<h2>Products</h2>`;
                products.forEach(product => {
                    content += `
                        <div class="product">
                            <img src="${product.img}" alt="${product.name}">
                            <div>
                                <h3>${product.name}</h3>
                                <p>Price: $${product.price}</p>
                            </div>
                            <button class="add-to-cart" onclick="addToCart(${product.id})">Add to Cart</button>
                        </div>
                    `;
                });
                mainContent.innerHTML = content;
            }

            function loadCart() {
                let content = `<h2>Shopping Cart</h2>`;
                if (cart.length === 0) {
                    content += `<p>Your cart is empty.</p>`;
                } else {
                    cart.forEach(item => {
                        const product = products.find(p => p.id === item.productId);
                        content += `
                            <div class="cart-item">
                                <img src="${product.img}" alt="${product.name}">
                                <div>
                                    <h3>${product.name}</h3>
                                    <p>Price: $${product.price}</p>
                                    <p>Quantity: ${item.quantity}</p>
                                </div>
                                <button class="remove" onclick="removeFromCart(${product.id})">Remove</button>
                            </div>
                        `;
                    });
                    content += `<button onclick="checkout()">Checkout</button>`;
                }
                mainContent.innerHTML = content;
            }

            function loadOrders() {
                let content = `<h2>Your Orders</h2>`;
                if (orders.length === 0) {
                    content += `<p>You have no orders.</p>`;
                } else {
                    orders.forEach(order => {
                        content += `
                            <div class="order-item">
                                <div>
                                    <h3>Order #${order.id}</h3>
                                    <p>Items:</p>
                                    <ul>
                                        ${order.items.map(item => {
                                            const product = products.find(p => p.id === item.productId);
                                            return `<li>${product.name} - Quantity: ${item.quantity}</li>`;
                                        }).join('')}
                                    </ul>
                                </div>
                            </div>
                        `;
                    });
                }
                mainContent.innerHTML = content;
            }

            function loadAccount() {
                let content = `<h2>Account</h2>`;
                if (user) {
                    content += `<p>Welcome, ${user.username}!</p>`;
                    content += `<button onclick="logout()">Logout</button>`;
                } else {
                    content += `
                        <form onsubmit="login(event)">
                            <label for="username">Username:</label>
                            <input type="text" id="username" required>
                            <label for="password">Password:</label>
                            <input type="password" id="password" required>
                            <button type="submit">Login</button>
                        </form>
                    `;
                }
                mainContent.innerHTML = content;
            }

            window.addToCart = function(productId) {
                const productInCart = cart.find(item => item.productId === productId);
                if (productInCart) {
                    productInCart.quantity++;
                } else {
                    cart.push({ productId, quantity: 1 });
                }
                alert('Product added to cart!');
            }

            window.removeFromCart = function(productId) {
                cart = cart.filter(item => item.productId !== productId);
                loadCart();
            }

            window.checkout = function() {
                const orderId = orders.length + 1;
                orders.push({id: orderId, items: [...cart] });
                cart = [];
                alert('Checkout successful!');
                loadOrders();
            }

            window.login = function(event) {
                event.preventDefault();
                const username = document.getElementById('username').value;
                const password = document.getElementById('password').value;
                // Simple authentication simulation
                if (username === 'user' && password === 'password') {
                    user = { username };
                    alert('Login successful!');
                    loadAccount();
                } else {
                    alert('Invalid username or password!');
                }
            }

            window.logout = function() {
                user = null;
                alert('Logout successful!');
                loadAccount();
            }
        });
    </script>
</body>
</html>

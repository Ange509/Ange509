from flask import Flask, render_template, request, redirect, url_for
import sqlite3

app = Flask(__name__)

def init_db():
    conn = sqlite3.connect('database.db')
    c = conn.cursor()
    c.execute('''CREATE TABLE IF NOT EXISTS products
                 (id INTEGER PRIMARY KEY, name TEXT, description TEXT, price REAL)''')
    conn.commit()
    conn.close()

@app.route('/')
def index():
    conn = sqlite3.connect('database.db')
    c = conn.cursor()
    c.execute('SELECT * FROM products')
    products = c.fetchall()
    conn.close()
    return render_template('index.html', products=products)

@app.route('/product/<int:product_id>')
def product(product_id):
    conn = sqlite3.connect('database.db')
    c = conn.cursor()
    c.execute('SELECT * FROM products WHERE id = ?', (product_id,))
    product = c.fetchone()
    conn.close()
    return render_template('product.html', product=product)

@app.route('/checkout/<int:product_id>')
def checkout(product_id):
    conn = sqlite3.connect('database.db')
    c = conn.cursor()
    c.execute('SELECT * FROM products WHERE id = ?', (product_id,))
    product = c.fetchone()
    conn.close()
    return render_template('checkout.html', product=product)

if __name__ == '__main__':
    init_db()
    app.run(debug=True)
    <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My E-commerce Site</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
</head>
<body>
    <h1>Welcome to My E-commerce Site</h1>
    <div>
        {% for product in products %}
        <div>
            <h2>{{ product[1] }}</h2>
            <p>{{ product[2] }}</p>
            <p>${{ product[3] }}</p>
            <a href="{{ url_for('product', product_id=product[0]) }}">View Product</a>
        </div>
        {% endfor %}
    </div>
</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ product[1] }}</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
</head>
<body>
    <h1>{{ product[1] }}</h1>
    <p>{{ product[2] }}</p>
    <p>${{ product[3] }}</p>
    <a href="{{ url_for('checkout', product_id=product[0]) }}">Buy Now</a>
    <a href="{{ url_for('index') }}">Back to Home</a>
</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Checkout - {{ product[1] }}</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
</head>
<body>
    <h1>Checkout</h1>
    <p>You are purchasing: {{ product[1] }}</p>
    <p>Total cost: ${{ product[3] }}</p>
    <form action="/complete_purchase" method="POST">
        <!-- Add form fields for user information -->
        <button type="submit">Complete Purchase</button>
    </form>
    <a href="{{ url_for('index') }}">Back to Home</a>
</body>
</html>
body {
    font-family: Arial, sans-serif;
    padding: 20px;
}

h1 {
    color: #333;
}

div {
    margin-bottom: 20px;
}

a {
    display: inline-block;
    margin-top: 10px;
    text-decoration: none;
    color: #0066cc;
}

a:hover {
    text-decoration: underline;
}
import sqlite3

conn = sqlite3.connect('database.db')
c = conn.cursor()
c.execute('''CREATE TABLE IF NOT EXISTS products
             (id INTEGER PRIMARY KEY, name TEXT, description TEXT, price REAL)''')
c.execute("INSERT INTO products (name, description, price) VALUES ('Product 1', 'Description for product 1', 19.99)")
c.execute("INSERT INTO products (name, description, price) VALUES ('Product 2', 'Description for product 2', 29.99)")
conn.commit()
conn.close()

### 💻 Simple code to demonstrate your progres.

```python
from flask import Flask, render_template, request, redirect, url_for

app = Flask(__name__)

# Temporary product data
products = [
    {
        "id": 1,
        "name": "Laptop",
        "price": 750,
        "stock": 5
    },
    {
        "id": 2,
        "name": "Wireless Mouse",
        "price": 20,
        "stock": 10
    },
    {
        "id": 3,
        "name": "Keyboard",
        "price": 35,
        "stock": 8
    }
]

cart = []


@app.route("/")
def home():
    return render_template("index.html", products=products)


@app.route("/add-to-cart/<int:product_id>")
def add_to_cart(product_id):
    for product in products:
        if product["id"] == product_id:
            cart.append(product)
            break

    return redirect(url_for("home"))


@app.route("/cart")
def view_cart():
    total = sum(item["price"] for item in cart)

    return render_template(
        "cart.html",
        cart=cart,
        total=total
    )


@app.route("/checkout", methods=["GET", "POST"])
def checkout():

    if request.method == "POST":
        customer_name = request.form["name"]
        address = request.form["address"]

        print("New Order")
        print("Customer:", customer_name)
        print("Delivery Address:", address)
        print("Delivery Method: FPS Home Delivery")

        cart.clear()

        return "Order placed successfully! FPS will deliver your products to your home."

    return render_template("checkout.html")


if __name__ == "__main__":
    app.run(debug=True)
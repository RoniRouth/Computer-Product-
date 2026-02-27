import React, { useState } from "react"; import { Card, CardContent } from "@/components/ui/card"; import { Button } from "@/components/ui/button"; import { Input } from "@/components/ui/input"; import { motion } from "framer-motion";

export default function ComputerStore() { const [cart, setCart] = useState([]); const [order, setOrder] = useState({ name: "", phone: "", address: "" }); const [isAdmin, setIsAdmin] = useState(false); const [adminPassword, setAdminPassword] = useState(""); const [products, setProducts] = useState([ { id: 1, name: "Gaming Laptop", price: 65000 }, { id: 2, name: "Desktop Computer", price: 45000 }, { id: 3, name: "Mechanical Keyboard", price: 2500 }, { id: 4, name: "Wireless Mouse", price: 1200 }, ]);

const [newProduct, setNewProduct] = useState({ name: "", price: "" });

const addToCart = (product) => { setCart([...cart, product]); };

const handleOrder = () => { const message = New Order:%0AName: ${order.name}%0APhone: ${order.phone}%0AAddress: ${order.address}%0ATotal: ₹${totalPrice}; window.open(https://wa.me/918172049400?text=${message}, "_blank"); setCart([]); setOrder({ name: "", phone: "", address: "" }); };

const totalPrice = cart.reduce((total, item) => total + item.price, 0);

const loginAdmin = () => { if (adminPassword === "admin123") { setIsAdmin(true); } else { alert("Wrong Password"); } };

const addProduct = () => { if (!newProduct.name || !newProduct.price) return; const product = { id: products.length + 1, name: newProduct.name, price: Number(newProduct.price), }; setProducts([...products, product]); setNewProduct({ name: "", price: "" }); };

return ( <div className="min-h-screen bg-gray-100 p-6"> <h1 className="text-3xl font-bold text-center mb-6">Computer Product Store</h1>

{/* Admin Login */}
  <div className="mb-6 text-center">
    {!isAdmin ? (
      <div className="flex justify-center gap-2">
        <Input
          type="password"
          placeholder="Admin Password"
          value={adminPassword}
          onChange={(e) => setAdminPassword(e.target.value)}
          className="max-w-xs"
        />
        <Button onClick={loginAdmin}>Login Admin</Button>
      </div>
    ) : (
      <p className="text-green-600 font-semibold">Admin Panel Activated</p>
    )}
  </div>

  {/* Admin Panel */}
  {isAdmin && (
    <div className="bg-white p-6 rounded-2xl shadow-md mb-10">
      <h2 className="text-2xl font-bold mb-4">Admin Panel - Add Product</h2>
      <div className="grid gap-4 md:grid-cols-3">
        <Input
          placeholder="Product Name"
          value={newProduct.name}
          onChange={(e) => setNewProduct({ ...newProduct, name: e.target.value })}
        />
        <Input
          placeholder="Price"
          type="number"
          value={newProduct.price}
          onChange={(e) => setNewProduct({ ...newProduct, price: e.target.value })}
        />
        <Button onClick={addProduct}>Add Product</Button>
      </div>
    </div>
  )}

  {/* Product Section */}
  <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
    {products.map((product) => (
      <motion.div key={product.id} whileHover={{ scale: 1.05 }}>
        <Card className="rounded-2xl shadow-lg">
          <CardContent className="p-4 text-center">
            <h2 className="text-xl font-semibold">{product.name}</h2>
            <p className="text-lg mt-2">₹{product.price}</p>
            <Button className="mt-4" onClick={() => addToCart(product)}>
              Add to Cart
            </Button>
          </CardContent>
        </Card>
      </motion.div>
    ))}
  </div>

  {/* Order Section */}
  <div className="mt-10 bg-white p-6 rounded-2xl shadow-md">
    <h2 className="text-2xl font-bold mb-4">Order Section</h2>
    {cart.length === 0 ? (
      <p>No products added.</p>
    ) : (
      <div>
        {cart.map((item, index) => (
          <p key={index}>
            {item.name} - ₹{item.price}
          </p>
        ))}
        <p className="font-bold mt-2">Total: ₹{totalPrice}</p>
      </div>
    )}

    <div className="mt-4 grid gap-4">
      <Input
        placeholder="Your Name"
        value={order.name}
        onChange={(e) => setOrder({ ...order, name: e.target.value })}
      />
      <Input
        placeholder="Phone Number"
        value={order.phone}
        onChange={(e) => setOrder({ ...order, phone: e.target.value })}
      />
      <Input
        placeholder="Delivery Address"
        value={order.address}
        onChange={(e) => setOrder({ ...order, address: e.target.value })}
      />
      <Button onClick={handleOrder} className="bg-green-600 hover:bg-green-700">
        Order via WhatsApp
      </Button>
    </div>
  </div>
</div>

); }

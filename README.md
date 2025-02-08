import React from 'react';
import { Menu, X, ChevronRight, ShoppingCart, Star, Shirt, Package, TrendingUp } from 'lucide-react';

// Types
interface Product {
  id: number;
  name: string;
  price: number;
  image: string;
  description: string;
}

interface CartItem extends Product {
  quantity: number;
}

function App() {
  const [isMenuOpen, setIsMenuOpen] = React.useState(false);
  const [isCartOpen, setIsCartOpen] = React.useState(false);
  const [cart, setCart] = React.useState<CartItem[]>([]);

  // Produits
  const products: Product[] = [
    {
      id: 1,
      name: "T-Shirt Premium Noir",
      price: 149.99,
      image: "https://images.unsplash.com/photo-1583743814966-8936f5b7be1a?auto=format&fit=crop&w=800&q=80",
      description: "T-shirt en coton organique de luxe"
    },
    {
      id: 2,
      name: "T-Shirt Collection Or",
      price: 199.99,
      image: "https://images.unsplash.com/photo-1521572163474-6864f9cf17ab?auto=format&fit=crop&w=800&q=80",
      description: "Édition limitée avec broderie or"
    },
    {
      id: 3,
      name: "T-Shirt Signature Blanc",
      price: 169.99,
      image: "https://images.unsplash.com/photo-1581655353564-df123a1eb820?auto=format&fit=crop&w=800&q=80",
      description: "Notre bestseller signature"
    }
  ];

  const addToCart = (product: Product) => {
    setCart(prevCart => {
      const existingItem = prevCart.find(item => item.id === product.id);
      if (existingItem) {
        return prevCart.map(item =>
          item.id === product.id
            ? { ...item, quantity: item.quantity + 1 }
            : item
        );
      }
      return [...prevCart, { ...product, quantity: 1 }];
    });
    setIsCartOpen(true);
  };

  const removeFromCart = (productId: number) => {
    setCart(prevCart => prevCart.filter(item => item.id !== productId));
  };

  const updateQuantity = (productId: number, newQuantity: number) => {
    if (newQuantity < 1) return;
    setCart(prevCart =>
      prevCart.map(item =>
        item.id === productId
          ? { ...item, quantity: newQuantity }
          : item
      )
    );
  };

  const total = cart.reduce((sum, item) => sum + item.price * item.quantity, 0);

  return (
    <div className="min-h-screen bg-white">
      {/* Navigation */}
      <nav className="bg-black text-white shadow-lg fixed w-full z-50">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
          <div className="flex justify-between h-16">
            <div className="flex items-center">
              <span className="text-2xl font-bold">Luxury Shop</span>
            </div>
            
            {/* Desktop Navigation */}
            <div className="hidden md:flex items-center space-x-8">
              <a href="#accueil" className="text-white hover:text-gold transition-colors">Accueil</a>
              <a href="#collection" className="text-white hover:text-gold transition-colors">Collection</a>
              <a href="#apropos" className="text-white hover:text-gold transition-colors">À Propos</a>
              <a href="#contact" className="text-white hover:text-gold transition-colors">Contact</a>
              <button 
                onClick={() => setIsCartOpen(true)}
                className="relative hover:text-gold transition-colors"
              >
                <ShoppingCart className="h-6 w-6" />
                {cart.length > 0 && (
                  <span className="absolute -top-2 -right-2 bg-gold text-black text-xs font-bold px-2 py-1 rounded-full">
                    {cart.reduce((sum, item) => sum + item.quantity, 0)}
                  </span>
                )}
              </button>
            </div>

            {/* Mobile menu button */}
            <div className="md:hidden flex items-center space-x-4">
              <button 
                onClick={() => setIsCartOpen(true)}
                className="relative"
              >
                <ShoppingCart className="h-6 w-6 text-white" />
                {cart.length > 0 && (
                  <span className="absolute -top-2 -right-2 bg-gold text-black text-xs font-bold px-2 py-1 rounded-full">
                    {cart.reduce((sum, item) => sum + item.quantity, 0)}
                  </span>
                )}
              </button>
              <button
                onClick={() => setIsMenuOpen(!isMenuOpen)}
                className="text-white"
              >
                {isMenuOpen ? <X size={24} /> : <Menu size={24} />}
              </button>
            </div>
          </div>
        </div>

        {/* Mobile Navigation */}
        {isMenuOpen && (
          <div className="md:hidden bg-black">
            <div className="px-2 pt-2 pb-3 space-y-1">
              <a href="#accueil" className="block px-3 py-2 text-white hover:text-gold transition-colors">Accueil</a>
              <a href="#collection" className="block px-3 py-2 text-white hover:text-gold transition-colors">Collection</a>
              <a href="#apropos" className="block px-3 py-2 text-white hover:text-gold transition-colors">À Propos</a>
              <a href="#contact" className="block px-3 py-2 text-white hover:text-gold transition-colors">Contact</a>
            </div>
          </div>
        )}
      </nav>

      {/* Panier */}
      {isCartOpen && (
        <div className="fixed inset-0 z-50 overflow-hidden">
          <div className="absolute inset-0 bg-black bg-opacity-50" onClick={() => setIsCartOpen(false)} />
          <div className="absolute inset-y-0 right-0 max-w-full flex">
            <div className="relative w-screen max-w-md">
              <div className="h-full flex flex-col bg-white shadow-xl">
                <div className="flex-1 py-6 overflow-y-auto px-4 sm:px-6">
                  <div className="flex items-start justify-between">
                    <h2 className="text-lg font-medium text-gray-900">Panier</h2>
                    <button
                      onClick={() => setIsCartOpen(false)}
                      className="ml-3 h-7 flex items-center"
                    >
                      <X className="h-6 w-6 text-gray-400" />
                    </button>
                  </div>

                  <div className="mt-8">
                    {cart.length === 0 ? (
                      <p className="text-gray-500">Votre panier est vide</p>
                    ) : (
                      <div className="flow-root">
                        <ul className="-my-6 divide-y divide-gray-200">
                          {cart.map((item) => (
                            <li key={item.id} className="py-6 flex">
                              <div className="flex-shrink-0 w-24 h-24 overflow-hidden rounded-md">
                                <img
                                  src={item.image}
                                  alt={item.name}
                                  className="w-full h-full object-cover"
                                />
                              </div>
                              <div className="ml-4 flex-1 flex flex-col">
                                <div>
                                  <div className="flex justify-between text-base font-medium text-gray-900">
                                    <h3>{item.name}</h3>
                                    <p className="ml-4">{item.price.toFixed(2)} €</p>
                                  </div>
                                </div>
                                <div className="flex-1 flex items-end justify-between text-sm">
                                  <div className="flex items-center">
                                    <button
                                      onClick={() => updateQuantity(item.id, item.quantity - 1)}
                                      className="px-2 py-1 border rounded"
                                    >
                                      -
                                    </button>
                                    <span className="mx-2">{item.quantity}</span>
                                    <button
                                      onClick={() => updateQuantity(item.id, item.quantity + 1)}
                                      className="px-2 py-1 border rounded"
                                    >
                                      +
                                    </button>
                                  </div>
                                  <button
                                    type="button"
                                    onClick={() => removeFromCart(item.id)}
                                    className="font-medium text-red-600 hover:text-red-500"
                                  >
                                    Supprimer
                                  </button>
                                </div>
                              </div>
                            </li>
                          ))}
                        </ul>
                      </div>
                    )}
                  </div>
                </div>

                <div className="border-t border-gray-200 py-6 px-4 sm:px-6">
                  <div className="flex justify-between text-base font-medium text-gray-900">
                    <p>Total</p>
                    <p>{total.toFixed(2)} €</p>
                  </div>
                  <p className="mt-0.5 text-sm text-gray-500">
                    Frais de livraison calculés à la commande.
                  </p>
                  <div className="mt-6">
                    <button
                      className="w-full flex justify-center items-center px-6 py-3 border border-transparent rounded-md shadow-sm text-base font-medium text-white bg-black hover:bg-gray-900"
                      onClick={() => alert('Paiement en cours de développement')}
                    >
                      Commander
                    </button>
                  </div>
                  <div className="mt-6 flex justify-center text-sm text-center text-gray-500">
                    <p>
                      ou{' '}
                      <button
                        type="button"
                        className="font-medium text-black hover:text-gray-800"
                        onClick={() => setIsCartOpen(false)}
                      >
                        Continuer vos achats
                      </button>
                    </p>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      )}

      {/* Hero Section */}
      <section id="accueil" className="hero-pattern relative">
        <div className="relative z-10 max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-32 pt-32">
          <div className="text-center">
            <h1 className="text-4xl tracking-tight font-extrabold sm:text-5xl md:text-6xl">
              <span className="block">LUXURY SHOP</span>
              <span className="block text-gold">Collection Premium</span>
            </h1>
            <p className="mt-3 max-w-md mx-auto text-base text-gray-300 sm:text-lg md:mt-5 md:text-xl md:max-w-3xl">
              Découvrez notre collection exclusive de vêtements haut de gamme. Qualité exceptionnelle, style intemporel.
            </p>
            <div className="mt-5 max-w-md mx-auto sm:flex sm:justify-center md:mt-8">
              <div className="rounded-md shadow">
                <a href="#collection" className="w-full flex items-center justify-center px-8 py-3 border border-transparent text-base font-medium rounded-md text-black bg-gold hover:bg-gold/90 md:py-4 md:text-lg md:px-10">
                  Voir la Collection
                </a>
              </div>
            </div>
          </div>
        </div>
      </section>

      {/* Collection Section */}
      <section id="collection" className="py-20 luxury-pattern">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
          <div className="text-center">
            <h2 className="text-3xl font-extrabold text-white sm:text-4xl">
              Notre Collection
            </h2>
            <p className="mt-4 text-xl text-gray-300">
              Des pièces uniques, créées avec passion
            </p>
          </div>

          <div className="mt-20 grid grid-cols-1 gap-8 sm:grid-cols-2 lg:grid-cols-3">
            {products.map((product) => (
              <div key={product.id} className="relative group bg-white rounded-lg overflow-hidden shadow-xl transform transition-transform duration-300 hover:-translate-y-2">
                <div className="relative w-full h-80 rounded-t-lg overflow-hidden">
                  <img
                    src={product.image}
                    alt={product.name}
                    className="w-full h-full object-cover object-center transform transition-transform duration-300 group-hover:scale-105"
                  />
                  <div className="absolute inset-0 bg-black bg-opacity-0 group-hover:bg-opacity-30 transition-opacity duration-300" />
                </div>
                <div className="p-6">
                  <h3 className="text-lg font-medium text-gray-900">{product.name}</h3>
                  <p className="mt-1 text-sm text-gray-500">{product.description}</p>
                  <div className="mt-4 flex justify-between items-center">
                    <p className="text-xl font-bold text-gray-900">{product.price.toFixed(2)} €</p>
                    <button
                      onClick={() => addToCart(product)}
                      className="bg-black text-white px-4 py-2 rounded-md hover:bg-gray-900 transition-colors duration-200"
                    >
                      Ajouter au panier
                    </button>
                  </div>
                </div>
              </div>
            ))}
          </div>
        </div>
      </section>

      {/* Avantages Section */}
      <section className="py-20 bg-white">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
          <div className="grid grid-cols-1 gap-8 md:grid-cols-3">
            {[
              {
                icon: <Package className="h-8 w-8 text-gold" />,
                title: "Livraison Premium",
                description: "Livraison gratuite dans le monde entier"
              },
              {
                icon: <Shirt className="h-8 w-8 text-gold" />,
                title: "Qualité Garantie",
                description: "Matériaux haut de gamme sélectionnés"
              },
              {
                icon: <TrendingUp className="h-8 w-8 text-gold" />,
                title: "Éditions Limitées",
                description: "Collections exclusives et numérotées"
              }
            ].map((feature, index) => (
              <div key={index} className="text-center transform transition-transform duration-300 hover:-translate-y-2">
                <div className="flex items-center justify-center h-12 w-12 rounded-md bg-black mx-auto">
                  {feature.icon}
                </div>
                <h3 className="mt-4 text-xl font-medium text-gray-900">{feature.title}</h3>
                <p className="mt-2 text-base text-gray-500">{feature.description}</p>
              </div>
            ))}
          </div>
        </div>
      </section>

      {/* Contact Section */}
      <section id="contact" className="py-20 luxury-pattern">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
          <div className="text-center">
            <h2 className="text-3xl font-extrabold text-white sm:text-4xl">
              Contactez-nous
            </h2>
            <p className="mt-4 text-xl text-gray-300">
              Notre équipe est à votre disposition
            </p>
          </div>

          <div className="mt-20">
            <form className="max-w-xl mx-auto space-y-6 bg-white p-8 rounded-lg shadow-xl">
              <div>
                <label htmlFor="name" className="block text-sm font-medium text-gray-700">
                  Nom
                </label>
                <input
                  type="text"
                  name="name"
                  id="name"
                  className="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-gold focus:ring-gold"
                />
              </div>
              <div>
                <label htmlFor="email" className="block text-sm font-medium text-gray-700">
                  Email
                </label>
                <input
                  type="email"
                  name="email"
                  id="email"
                  className="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-gold focus:ring-gold"
                />
              </div>
              <div>
                <label htmlFor="message" className="block text-sm font-medium text-gray-700">
                  Message
                </label>
                <textarea
                  id="message"
                  name="message"
                  rows={4}
                  className="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-gold focus:ring-gold"
                />
              </div>
              <div>
                <button
                  type="submit"
                  className="w-full flex justify-center py-2 px-4 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-black hover:bg-gray-900 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-gold"
                >
                  Envoyer
                </button>
              </div>
            </form>
          </div>
        </div>
      </section>

      {/* Footer */}
      <footer className="bg-black">
        <div className="max-w-7xl mx-auto py-12 px-4 sm:px-6 lg:px-8">
          <div className="text-center text-white">
            <p className="text-base">
              © 2025 Luxury Shop. Tous droits réservés.
            </p>
          </div>
        </div>
      </footer>
    </div>
  );
}

export default App;

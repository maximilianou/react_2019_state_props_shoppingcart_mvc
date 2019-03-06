---

## React using state and props, Shopping Cart MVC 2019

https://codesandbox.io/s/v0l1knv1yl

I mean, the simplest example learning how to use the React state, and React props

( Clearly here i'm not using Redux, this is the next step, just to be clear )

React state of the App

React props of each React.Component Instance

So we can have list of products, and a shopping cart (list of items of products), adding and removing items.

The point is to learn how to pass action control over components, changing from state to new states on action events from components, to the Root App, and showing the change in the other affected Component.

```javascript

import React from "react";
import ReactDOM from "react-dom";

import "./styles.css";

const ProductsList = props => {
  const { products, adding } = props;
  return (
    <section>
      <h2>List of Products</h2>
      {products.map(product => (
        <article key={"product_" + product.id}>
          <h4>{product.name}</h4>
          <span>{product.price}</span>
          <button onClick={adding} value={JSON.stringify(product)}>
            +
          </button>
        </article>
      ))}
    </section>
  );
};
const ItemsList = props => {
  const { items, removing } = props;
  return (
    <section>
      {items.map(item => (
        <article key={"item_" + item.product.id}>
          <h4>{item.product.name}</h4>
          <span>{item.product.price}</span>
          <span>{item.quantity}</span>
          <span>{item.quantity * item.product.price}</span>
          <button onClick={removing} value={JSON.stringify(item)}>
            -
          </button>
        </article>
      ))}
    </section>
  );
};
class ShoppingCart extends React.Component {
  constructor(props) {
    super(props);
  }
  render() {
    return (
      <section>
        <h2>Shopping Cart</h2>
        <ItemsList items={this.props.items} removing={this.props.removing} />
      </section>
    );
  }
}
class App extends React.Component {
  state = {
    products: [
      { id: 20, name: "Limonade", price: 200 },
      { id: 21, name: "Green Tea", price: 150 },
      { id: 22, name: "Chocolate Milk", price: 350 }
    ],
    items: []
  };
  constructor(props) {
    super(props);
    this.adding = this.adding.bind(this);
    this.removing = this.removing.bind(this);
  }
  adding = evt => {
    const product = JSON.parse(evt.target.value);
    let item = this.state.items.find(ite => ite.product.id === product.id);
    if (item === null || item === undefined) {
      item = {};
      item.product = product;
    }
    if (item.quantity === undefined) {
      item.quantity = 1;
      this.setState({ items: [...this.state.items, item] });
    } else {
      item.quantity += 1; // mm.. here i'm changing the original object. mmm...
      this.setState({ items: [...this.state.items] });
    }
  };
  removing = evt => {
    const item_p = JSON.parse(evt.target.value);
    const item = this.state.items.find(
      ite => ite.product.id === item_p.product.id
    );
    if (item.quantity === 1) {
      const listItems = this.state.items.filter(
        ite => ite.product.id !== item.product.id
      );
      this.setState({
        items: listItems
      });
    } else {
      item.quantity -= 1; // mmm.. here i'm changing the original object. mmm...
      this.setState({ items: [...this.state.items] });
    }
  };
  render() {
    return (
      <div className="App">
        <ProductsList products={this.state.products} adding={this.adding} />
        <ShoppingCart items={this.state.items} removing={this.removing} />
      </div>
    );
  }
}

const rootElement = document.querySelector("#root");
ReactDOM.render(<App />, rootElement);



```
Cart.js
=======

Cart.js is a basic javascript cart using JSON with localStorage as
a persistence mechanism.

Install
-------

Just copy the file and source it in your html.
Then init the cart.

```html
<script type="text/javascript" src="cart.js"></script>
<script type="text/javascript">
  Cart.init();
</script>
```

Everything is under the `Cart` namespace.

This gives you the cart functionalities only, but there is also another init 
function if you use JQuery which also attach events for you.

```html
<script type="text/javascript" src="jquery.js"></script>
<script type="text/javascript" src="cart.js"></script>
<script type="text/javascript">
  $(function() {
    Cart.initJQuery();
  });
</script>
```

You'll find documentation for JQuery further.

Functions
---------

`Cart.on(eventName, callback)` lets you attach a callback function to an event 
on the cart. You can add custom events but the provided events are: `"added"`, 
`"removed"`, `"saved"` and `"emptied"`.

```javascript
Cart.on('added', function(argumentsObject) {
  alert("You've added " + argumentsObject.item.quantity + " item(s).");
});
```

`Cart.addItem(item)` adds the item to the cart. The item is an object with 
the fields you want to save for an item, but at least these ones: `id`, 
`quantity`, `price`, `label`. Price is the unit price in cents/pence, 
therefore an integer.

```javascript
Cart.addItem({
  id: '123456',
  price: 499,
  quantity: 5,
  label: 'Water based marker',
  image: "/img/product/123456.jpg"
});
```

By default, if you don't pass a quantity, it will be assumed to be `1`.

If you just want to increase the quantity of an item, you can use the same 
function and only put the `id` and the `quantity` in the object. You can also 
decrease the quantity if you give a negative number.

```javascript
Cart.addItem({ id: '123456', quantity: -1 });
```

`Cart.empty()` removes all items from the cart.

`Cart.indexOfItem(id)` returns the index of an item if you know its ID.

`Cart.itemsCount()` returns the total number of items in the cart.

`Cart.subTotal()` returns the subtotal of what is in the cart. That is just the 
price of all the items, not including shipping or taxes.

`Cart.displayPrice(price)` returns a formated version of the price if you need 
to print it on the page. By default the currency sign is `&pound;` but you can 
change this variable. It returns `"Free"` if the price is `0`.

```javascript
Cart.currency = 'USD';
Cart.displayPrice(499); // returns "USD 4.99"
Cart.displayPrice(0);   // returns "Free"
```

`Cart.init()` initializes the cart.

Using the cart with jQuery
--------------------------

If you are using jQuery, then you can use the `initJQuery` function as opposed 
to the normal `init`. It is preferable to call it when the DOM is ready.

```javascript
$(function(){
  Cart.initJQuery();
});
```

Then you can give elements of your page classes to give them a functionality. 
The main one being `.cart-add`. The fields are passed by `data-` attributes. 

```html
<button 
    type='button' 
    class='cart-add' 
    data-id='123456' 
    data-label='Water based marker' 
    data-price='499'
    data-image='/img/product/123456.jpg'
  >
  Add to cart
</button>
```

Like with the `addItem` function, you can ignore quantity if you want `1`, you 
can just pass `id` and `quantity` and/or a negative number if you want to 
change the quantity of an item you already have in your cart.

Then for reporting what is in your cart, you have a few other CSS classes. 
`.cart-items-count`, `.cart-is-empty` and `.cart-subtotal` are quite obvious. Then you can show or
hide elements depending on if the number of total items is one or many using 
`.cart-items-count-singular` and `.cart-items-count-plural`. Here is a complete 
example:

```html
<div>
  <span class='cart-items-count'>0</span> 
  item<span class='cart-items-count-plural'>s</span> 
</div>
```

The full cart report is considered a table, but the template for line items 
can be changed with `Cart.lineItemTemplate` variable. Here is a complete 
example of a full cart report.

```html
<p>Cart: <span class='cart-items-count'>0</span> item<span class='cart-items-count-plural'>s</span></p>
<table class='cart-table'>
  <thead>
    <tr>
      <th>Image</th>
      <th>Product</th>
      <th>quantity</th>
      <th></th>
      <th>Price</th>
    </tr>
  </thead>
  <tbody class='cart-line-items'>
  </tbody>
  <tfoot>
    <tr>
      <td colspan='4'>Subtotal</td>
      <td class='cart-subtotal'></td>
    </tr>
  </tfoot>
</table>
<p class='cart-is-empty'>Your cart is empty.</p>
```

It then will show the table or `.cart-is-empty` depending on if you have 
something in the cart or not. And each line item will have buttons for changing 
quantities.


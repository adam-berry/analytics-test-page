# Bolt Support Engineer - Case Study - WIP

## Getting Started

- Clone this repository
- Install npm
- Run npm Build && npm start 
- The application will be running at http://localhost:3000

## How it works

- The Bolt scripts along with Google Analytics is loaded in the <head> tag in template.html
- The bulk of the project exists in src > routes > index
- When a user clicks on a name card in the gallery view, an onclick event handler calls updateCart() the item data and 'push' parameter. This creats a new object, adds this the the items array in the cart object and calls the BoltCheckout.configure() method.
- When the user clicks on the photo and closes the image, updateCart() is called with a 'pop' parameter which removes the item from items array in the cart and calls BoltCheckout.configure()



# source_for_website

### Carousel
- [SlickJS](http://kenwheeler.github.io/slick/)

![Preview of SlickJS](https://tips.zoego.tech/wp-content/uploads/tools_slick02.png)

### Cascading Grid Layout
#### With animation
- [MasonryJS](https://masonry.desandro.com/)

![Preview](http://i.imgur.com/iFBSB1t.jpg)

#### Without animation
- [BrickJS](http://callmecavs.com/bricks.js/)

![Preview](https://cdn.freebiesbug.com/wp-content/uploads/2016/02/bricks-js-masonry-580x435.jpg)

### Images
#### Load Images
- [ImagesLoaded](https://imagesloaded.desandro.com/)

### Fade Animation
- [AOS](https://github.com/michalsnik/aos) [Angular Import](https://stackoverflow.com/a/47291014)
  ```
  npm install aos --save
  ```
  In angular.json
  ```
  "styles": [
  "node_modules/aos/dist/aos.css",
  "styles.scss"
  ]
  ```
  In app.component.ts
  ```
  import * as AOS from 'aos';
  
  export class AppComponent implements OnInit {
    title = 'app';

    ngOnInit() {
      AOS.init();
    }
  }
  ```
  ```
  Note: 
  Throttle: Call once ONLY every N second
  Debounce: During N second first called, if no further call, call the last call.
  ```

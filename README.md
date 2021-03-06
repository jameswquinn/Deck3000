![alt text](https://image.ibb.co/diKhV8/deck3k.png "Deck3000")

**A slider component for the new millennium.**

## What it is
Simple! It's a slider component 😀 which can navigate (infinitely!) vertically ("sections") and horizontally ("slides", within a current section) in pure javascript.

What this component provides is a way to handle the basic and essential interactions of the slider itself. This will not provide you with the UI or anything like that; all the fun and responsibility of implementing the UI and interaction around the slider is all up to you!

![alt text](https://s22.postimg.cc/bus8dzp41/ezgif-1-7c340b3d99.gif "Deck3000 simple sample")

## Features
- Weighing in at a cute `~2kb` (js) `450b` (css)
- Infinite slides (vertical / horizontal)
- Scroll/Keyboard/Touch friendly
- Super smooth
- Super simple
- Super cute

## Coming soon
- [x] Navigation updates URL and history (replaceState)


## Getting Started

Include the module via `npm i deck3000` or `yarn add deck3000` and import it into your project.

```js
    import Deck3000 from 'deck3000';
```

**Very important** You must also include the [css](https://github.com/ezekielaquino/Deck3000/blob/master/dist/deck3000.css) that comes with the module!

```css
    // stylus, sass, etc.
    @import 'deck3000/dist/deck3000.css';
```

### DOM Structure
**Deck3000** requires the following structure from your HTML (see below)
- a parent container that includes the selector `js-deck3000`
- sections must be a direct descendant of the parent container
- slides must be a direct descendant of a section

*Note*: if you plan on having elements that are siblings of a slide within a section (for example,some absolute positioned elements within a section) you *MUST* have a selector on each slide and declare that as a `slideSelector` option when instantiating **Deck3000**, otherwise *all* direct descendants of a section will be instantiated as a slide.

After **Deck3000** successfully instantiated, a `is-init` selector will be attached to the parent `js-deck3000` element.

Animation speed and easing are all done via [CSS](https://github.com/ezekielaquino/Deck3000/blob/5ac9c62b4b78e2b2ec27a05f45533c58dca27cb2/dist/deck3000.css#L21) in addition to all the essentials of making the component work so **don't forget to import the stylesheet!**

```html
    <div class="mySectionClass js-deck3000">
        <!-- First section -->
        <div class="section" data-title="Museum project">
            <div class="slide">
                Slide one
            </div>
            <div class="slide">
                Slide two
            </div>
            <div class="slide">
                Slide three
            </div>
        </div>

        <!-- Second section -->
        <div class="section" data-title="Fashion editorial">
            <div class="slide">
                Section two - Slide one
            </div>
            <div class="slide">
                Section two - Slide two
            </div>
            <div class="slide">
                Section two - Slide three
            </div>
        </div>
    </div>
```

```js
    const Slideshow = new Deck3000({
        slideSelector: '.slide', // (optional)
        resetSlides: true,
        keyboardEvents: true,
        updateURL: true,
        onInit: state => func,
        onSectionStart: state => onSectionStart,
        onSectionEnd: state => func,
        onSlideStart: state => func,
        onSlideEnd: state => func,
    });

    const onSectionStart = state => {
        console.log(state); // This logs the state object below
    };
```

**Callbacks exposes `state`**
```js
    {
        currentSectionElem: DOMNode, // current section
        currentSlideElem: DOMNode, // currentSlide,
        section: {
            current: 1,
            direction: 'next',
            isAnimating: false,
            next: 2,
            prev: 0,
            sectionLength: 3, // This is actually length - 1
        },
        slide: {
            currentSlide: 0,
            next: 1,
            prev: 2,
            slideLength: 2, // This is actually length - 1
        }
    }
```

After **Deck3000** instantiates, it will not mutate the structure of the DOM in any way aside from adding classes to `sections` and `slides`.

## URLs

If you have set `updateURL` to `true` then you may attach a `data-title="Whatever"` to a `section`. This string will be converted into a slug e.g. `Fashion brand` -> `fashion-brand` and will be appended to the URL `/fashion-brand` when you navigate across sections. If you do set `updateURL` to `true` but do *not* attach a data-title attribute, then it will just use the index of the current section e.g. `/0`.

## API

#### navigate('type', 'direction') | Navigate through sections/slides.

a.k.a The ✨🥩 of **Deck3000**

`[Deck3000Instance].navigate(['section'/'slide'], ['next'/'prev']);`

```js
    const nextSlideBtn = document.querySelector('.nextSlideBtn');

    nextSlideBtn.addEventListener('click', () => {
        Slideshow.navigate('slide', 'next');
    });
```

#### disableEvents(bool) | Toggles all (scroll/keyboard/swipe) events.

`[Deck3000Instance].disableEvents(bool);`


```js
    // One might want to disable scroll events
    // (for ultimate peace of mind)
    // during certain UI states of your site/application
    const toggleOverlay = bool => {
        if (bool) {
            overlay.classList.add('is-active');
            Slideshow.disableEvents(bool);
            return;
        }

        overlay.classList.remove('is-active');
    };
```

#### Emitter
**Deck3000** uses `mitt` for handling events. The Emitter is exposed so you can subscribe to `navigate` (section) and `navigateSlide` (slide) events if you wish.

```js
    [Deck3000Instance].Emitter.on('navigate', e => {
        // Do something when navigating sections
        console.log(e);
    });

    [Deck3000Instance].Emitter.on('navigateSlide', e=> {
        // Do something when navigating slides
        console.log(e);
    });
```

## Thanks
- Built with [Microbundle](https://github.com/developit/microbundle) **It's awesome** 💃
- Also built with [Parcel](https://parceljs.org/) 🕺

## License
[MIT](https://oss.ninja/mit)

# Sole&Ankle — Module 4 workshop

In this workshop, our goal is to finish building an e-commerce store!

The good news is, most of our work is done already. We just need to write some additional CSS to construct the layout; things are a bit messy right now!

- Access the Figma: https://www.figma.com/file/kAL3AumTUV11y1IqHhltB6/Sole-and-Ankle-%E2%80%94-Mockup

This project uses Create React App. To get started, run the following terminal commands:

- `npm install`
- `npm run start`

You can then visit the app in-browser; it defaults to http://localhost:3000.

_Note that we're only focusing on the design._ The links and inputs don't do anything.

> **Want a bigger challenge?**
>
> This workshop comes with a lot of starter code — we'll be adding
> Flex-specific properties, but for the most part, we don't have a
> ton of code to write. If you'd prefer, you can build the app from
> scratch, to practice all the CSS we've learned so far!
>
> If you go that route, you can find the sneaker assets you need in
> `/public/assets`, and their metadata in `/src/data.js`. Design
> tokens can be found in `/src/constants`. The custom font is
> Raleway, from Google Fonts.

## Troubleshooting

If you run into problems running a local development server, check out our [Troubleshooting Guide](https://courses.joshwcomeau.com/troubleshooting) on the course platform.

This guide addresses the common `Digital Envelope Routine` error you may have seen.

---

## Exercise 1: Superheader

Let's build the “Superheader” a thin grey strip that runs along the top of the page:

![Close-up screenshot of the superheader](./docs/exercise-1-solution.png)

Use Flexbox to correctly align the elements within `src/components/SuperHeader`.

## Exercise 2: Header

Continuing on down, let's tackle the main header:

![Close-up screenshot of the header and superheader](./docs/exercise-2-solution.png)

The trickiest part of this exercise is the _position of the main navigation_. We want it to be perfectly centered within the container:

![Screenshot showing the position of the navigation within the header](./docs/nav-position.png)

This is a thorny problem, and it's not something we've explicitly seen in the course. Give it your best shot, but please don't be discouraged if you can't figure it out!

## Exercise 3: Shell

Next up, we want to tackle the "framing" around the shoe grid — the sidebar and title/filter.

![Screenshot of the store, with everything except the sneaker grid](./docs/exercise-3-solution.png)

_NOTE:_ To make life a bit easier, you may wish to comment out the `<ShoeGrid>` component. We'll work on integrating it in the next exercise.

## Exercise 4: Shoe Grid

This exercise features two mini-challenges. The second one is a chance to revisit some of the lessons learned in previous modules, and isn't as specific to Flexbox.

### 4A: Grid layout

Time to tackle the main feature of this application, the shoes!

Here's a screenshot of the final result:

![Screenshot of the store, with sneaker grid](./docs/exercise-4a-solution.png)

This is a tricky problem to solve with Flexbox—CSS Grid is a better tool for this job! Nevertheless, it can be done using Flexbox, with one caveat: the last row may be oversized:

![Screenshot of the shoe grid with one enormous sneaker, spanning 4 typical columns](./docs/giant-sneaker.png)

In a future module, we'll revisit this and see how CSS Grid can help us out :)

## 4B: Final touches

Our sneaker store is in pretty good shape, but there's two small details missing:

1. The “Sale” and “Just Released” flags.
1. The crossed-out prices, for items that are on sale.

Inside `ShoeCard.js`, you'll find a `variant` variable you can use to figure out which flag, if any, needs to be rendered. It's up to you to create the flag, using styled-components.

For the crossed-out prices, you can use the `price` and `salePrice` props.

![Screenshot of the store, with the final details added](./docs/exercise-4b-solution.png)

_NOTE:_ This exercise has minimal flexbox implications, and is mainly about revisiting lessons learned in the previous modules (including positioned layout and styled-components). Feel free to skip it if you'd prefer!

## To be continued!

Our sneaker store can flex to support different screen sizes, but there isn't a proper mobile or tablet view. Don't fret — we will revisit this workshop in a future module!

In the meantime, take a moment to congratulate yourself for making it through the Flexbox module!!

## Note to self

- `margin-right: auto` is a useful way of pushing content to the right of an element as far as possible.

- With the second exercise, the temptation is to make the logo have absolute positioning and take it out of the flow of the page and then use `justify-content: center` on the header. But one issue here is that when the page is squeezed, there'll be this overlap.

  So, the _Flexbox solution_ is to create a `<div>` component to live on the right of the header, and make both that and the logo have `flex: 1`, which sets `flex-grow: 1` but also `flex-basis: 0;`. This gives the logo no width, but it, along with the empty div, will both take up an equal amount of space on each side. This effectively squeezes the nav links into the centre.

- `align-items: baseline` is kinda magical. For this third exercise, it was used across three flex containers and the overall effect is that there is this straight line running underneath all the text (of different sizes)!

- Dynamically styling a component. There are two approaches.

  1. Creating an object of 'options', e.g.

     ```
     const SIZES = {
      small: {
        radius: 4,
        fontSize: 16,
        ...
      },
      medium: {
        ...
      }
     }
     ```

     Then in our functional component we can use whatever the variant is (in this case, the `size`) to pluck the options we want:

     ```
     const style = SIZES[size];
     ```

     These options can be used to create CSS variables and pass it to the component that consumes it:

     ```
     <SomeComponent style={{
      '--border-radius': style.radius + 'px',
      '--font-size': style.fontSize + 'px',
      ...
     }}>
     ```

     Finally, in our styled component, we use these CSS variables:

     ```
     const SomeComponent = styled.div`
       ...
       border-radius: var(--border-radius);
       font-size: var(--font-size);
     `;
     ```

  2. Using composition, e.g. in our JSX we might have this:

     ```
     {size === 'small' && <SmallComponent>}
     {size === 'medium' && <MediumComponent>}
     ```

     then we'd have one styled component to serve as a sort of base:

     ```
     const BaseComponent = styled.div`
       color: blue,
       ...
     `
     ```

     and we have the two components that use it as a base, inherit all its CSS:

     ```
     const SmallComponent = styled(BaseComponent)`
       font-size: ${SIZES.small[fontSize] + 'px'}
       border-radius: ${SIZES.small[radius] + 'px'}
     `
     ```

     Alternatively, we could instead take a different approach with the logic and JSX:

     ```
     const style = SIZES[size];

     let Component;
     if (size === 'small') {
      Component = SmallComponent;
     } else if (size === 'medium') {
      Component = MediumComponent;
     }
     ...
     <Component style={{ styles }}>
     ```

     where `SIZES` would be changed to use CSS variables as keys. Then our CSS would look a bit better because it would use CSS variables rather than interpolation.

- a brief mention of two different ways the styled component `Price` could be styled:

  1. Approach 1 (Props and Logic in CSS):

     ```
     <Price onSale={variant === 'on-sale'}>
       {formatPrice(price)}
     </Price>
     ...
     const Price = styled.span`
       text-decoration: ${(p) => p.onSale && 'line-through'};
       color: ${(p) => p.onSale && COLORS.gray[700]};
     `;
     ```

  2. Approach 2 (CSS Variables and Logic in JSX):

     ```
     <Price
       style={{
         '--color': variant === 'on-sale'
           ? COLORS.gray[700]
           : undefined,
         '--text-decoration': variant === 'on-sale'
           ? 'line-through'
           : undefined,
       }}
     >
       {formatPrice(price)}
     </Price>
     ...
     const Price = styled.span`
       color: var(--color);
       text-decoration: var(--text-decoration);
     `;
     ```

     The second approach is better, because the logic is separated from the CSS, making the CSS cleaner and more maintainable. Also, although we use the `style` prop, we are not doing any inline styles. By using CSS variables, we keep the styles in the CSS, which maintains the separation of concerns.

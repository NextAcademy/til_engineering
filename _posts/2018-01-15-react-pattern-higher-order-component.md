---
layout: post
title: "React Pattern: Higher Order Component"
author: "Liren"
date: 2018-01-15 00:00:00 +0800
tags: [react, javascript]
preview: https://i.imgur.com/2qHttje.jpg
---

#### Higher order what?
Higher Order Component (HOC) is simply a function that takes a component and returns a new powered-up component. It is not part of the React library, it's simply a pattern to write React.

#### Okaaay.. what for? (TLDR)
Reusability!

#### Fine, Give me the long version.

Components are usually categorized into a Smart (Container) or a Dumb (Presentational) one.

Smart components make API requests, connect to Redux store, do graphQL queries, etc. With all these shiny data, they pass them down to Dumb components as props. Dumb components will then display them *in style*.

Sometimes you have multiple versions of dumb components that need data from the same smart component, what do you do?

Perhaps you can just make a few identical copies of smart components and ~~Don't~~ Repeat Yourself.

Perhaps you can let the smart component passes all the data to Redux store and the dumb ones can get it from there, and suddenly the dumb components are not so dumb anymore - they can talk to The Store.

Or perhaps you should also consider HOC! or **[spoiler]** *Render Props*, that's the blog post for next month ;)

#### Example please!

Alright! Scenario:

>When client's browser is too wide, we want to alert them with our `<WidthAlert>` component, and force them to make it smaller (makes no sense, I know.)

Our `<WidthAlert>` may look like this:
```js
const WidthAlert = ({minWidth, width}) => (
  <div>
    {width > minWidth
        ? <div className="bg-alert">Make your screen smaller!</div>
        : null
    }
  </div>
)
```

but sometimes it needs to be presented differently, in a fancy `<Card/>`:
```js
const WidthAlertCard = ({minWidth, width}) => (
  <Card>
    {width > minWidth
        ? Reduce your width by {width - minWidth}.
        : Perfect!
    }
  </Card>
)
```

The above are the *dumb components*, they need someone else to detect current window size!

We will create a HOC that takes one of the above, and merge it together with a smart component:

```js
const withResizeListener = (AlertComponent, minWidth) =>
  class extends React.Component {
    constructor() {
      super()
      this.state = { width: 0 }
    }

    componentDidMount() {
      this.setState(
        {width: window.innerWidth},
        window.addEventListener(
          "resize",
          ({ target }) =>
            this.setState({width: target.innerWidth})
        )
      )
    }

    render() {
      return (
        <AlertComponent
          {...this.props} //this is technically not needed in this example
          width={this.state.width}
        />
      )
    }
  }
```

and those `WidthAlert` component can be **enhanced** like so:
```js
const WidthAlertWithResizeListener = withResizeListener(WidthAlert, 300)
const WidthAlertCardWithResizeListener = withResizeListener(WidthAlertCard, 300)
```

*Tadaa!* Now we can use our really long name components: `<WidthAlertWithResizeListener/>` and `<WidthAlertCardWithResizeListener/>`.

Oh have I mentioned about this perk of HOC? To give you the opportunity to name your components in 256 characters.

<h1 align="center">React Native Copilot</h1>

<div align="center">
  <p align="center">
    <a href="https://semaphoreci.com/okgrow/react-native-co-pilot">
      <img src="https://semaphoreci.com/api/v1/okgrow/react-native-co-pilot/branches/master/shields_badge.svg" alt="Build Status" />
    </a>
    <a href="https://www.npmjs.com/package/@okgrow/react-native-copilot">
      <img src="https://img.shields.io/npm/v/@okgrow/react-native-copilot.svg" alt="NPM Version" />
    </a>
    <a href="https://www.npmjs.com/package/@okgrow/react-native-copilot">
      <img src="https://img.shields.io/npm/dm/@okgrow/react-native-copilot.svg" alt="NPM Downloads" />
    </a>
  </p>
</div>

<p align="center">
  Step-by-step walkthrough for your react native app!
</p>

<p align="center">
  <img src="https://media.giphy.com/media/65VKIzGWZmHiEgEBi7/giphy.gif" alt="React Native Copilot" />
</p>

<p align="center">
  <a href="https://expo.io/@mohebifar/copilot-example" >
    Demo
  </a>
</p>

## Installation
```
npm install --save @okgrow/react-native-copilot
```

**Optional**: If you want to have the smooth SVG animation, you should install and link `react-native-svg`. If you are using Expo, **you can skip** this as Expo comes with `react-native-svg`.

```
npm install --save react-native-svg
react-native link react-native-svg
```

## Usage
Use the `copilot()` higher order component for the screen component that you want to use copilot with:

```js
import { copilot } from '@okgrow/react-native-copilot';

class HomeScreen extends Component { /* ... */ }

export default copilot()(HomeScreen);
```

Before defining walkthrough steps for your react elements, you must make them `walkthroughable`. The easiest way to do that for built-in react native components, is using the `walkthroughable` HOC. Then you must wrap the element with `CopilotStep`.

```js
import { copilot, walkthroughable, CopilotStep } from '@okgrow/react-native-copilot';

const CopilotText = walkthroughable(Text);

class HomeScreen {
  render() {
    return (
      <View>
        <CopilotStep text="This is a hello world example!" order={1} name="hello">
          <CopilotText>Hello world!</CopilotText>
        </CopilotStep>
      </View>
    );
  }
}
```

Every `CopilotStep` must have these props:

1. **name**: A unique name for the walkthrough step.
2. **order**: A positive number indicating the order of the step in the entire walkthrough.
3. **text**: The text shown as the description for the step.

In order to start the tutorial, you can call the `start` prop function in the root component that is injected by `copilot`:

```js
class HomeScreen extends Component {
  handleStartButtonPress() {
    this.props.start();
  }

  render() {
    // ...
  }
}

export default copilot()(HomeScreen);
```

If you are looking for a working example, please check out [this link](https://github.com/okgrow/react-native-copilot/blob/master/example/App.js).

### Overlays and animation
The overlay in react-native copilot is the component that draws the dark transparent over the root component. React-native copilot comes with two overlay components: `view` and `svg`.

The `view` overlay uses 4 rectangles drawn around the target element using the `<View />` component. We don't recommend using animation with this overlay since it's sluggish on some devices specially on Android devices.

The `svg` overlay uses an SVG path component for drawing the overlay. It offers a nice and smooth animation but it depends on `react-native-svg`. If you are using expo, you don't need to install anything and the svg overlay works out of the box. If not, you need to install and this package:

```
npm install --save react-native-svg
react-native link react-native-svg
```

You can specify the overlay when applying the `copilot` HOC:

```js
copilot({
  overlay: 'svg', // or 'view'
  animated true, // or false
})(RootComponent);
```

### Custom tooltip component
You can customize the tooltip by passing a component to the `copilot` HOC maker. If you are looking for an example tooltip component, take a look at [the default tooltip implementation](https://github.com/okgrow/react-native-copilot/blob/master/src/components/Tooltip.js).

```js
const TooltipComponent = ({
  isFirstStep,
  isLastStep,
  handleNext,
  handlePrev,
  handleStop,
  currentStep,
}) => (
  // ...
);

copilot({
  tooltipComponent: TooltipComponent
})(RootComponent)
```

### Custom components as steps
The components wrapped inside `CopilotStep`, will receive a `copilot` prop of type `Object` which the outermost rendered element of the component or the element that you want the tooltip be shown around, must extend.

```js
import { copilot, CopilotStep } from '@okgrow/react-native-copilot';

const CustomComponent = ({ copilot }) => <View {...copilot}><Text>Hello world!</Text></View>;

class HomeScreen {
  render() {
    return (
      <View>
        <CopilotStep text="This is a hello world example!" order={1} name="hello">
          <CustomComponent />
        </CopilotStep>
      </View>
    );
  }
}
```

### Triggering the tutorial
Use `this.props.start()` in the root component in order to trigger the tutorial. You can either invoke it with a touch event or in `componentDidMount`. Note that the component and all its descendants must be mounted before starting the tutorial since the `CopilotStep`s need to be registered first.

## Contributing
Issues and Pull Requests are always welcome.

Please read OK GROW!'s global [contribution guidelines](https://okgrow.github.io/guides/docs/open-source-contributing.html).

If you are interested in becoming a maintainer, get in touch with us by sending an email or opening an issue. You should already have code merged into the project. Active contributors are encouraged to get in touch.

Please note that all interactions in @okgrow's repos should follow our [Code of Conduct](https://okgrow.github.io/guides/docs/open-source-code-of-conduct.html).

## License

[MIT](LICENSE) © 2017 OK GROW!, https://www.okgrow.com.

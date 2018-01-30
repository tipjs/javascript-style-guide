[원문: https://github.com/airbnb/javascript/tree/master/css-in-javascript](https://github.com/airbnb/javascript/tree/master/css-in-javascript)

# Airbnb CSS-in-JavaScript 스타일 가이드

*대체로 합리적인 CSS-in-JavaScript에 대한 접근 방법*

## 목차

1. [명명규칙 (Naming)](#명명규칙-naming)
1. [순서 (Ordering)](#순서-ordering)
1. [중첩 (Nesting)](#중첩-nesting)
1. [인라인 (Inline)](#인라인-inline)
1. [테마 (Themes)](#테마-themes)

## 명명규칙 (Naming)

  - Use camelCase for object keys (i.e. "selectors").
  - 객체 키에 캐멀케이스(camelCase)를 사용하세요. (i.e. "selectors").

    > Why? We access these keys as properties on the `styles` object in the component, so it is most convenient to use camelCase.

    > 왜? 우리는 키를 컴포넌트 내 `styles` 객체의 속성으로 사용하기 때문에, 캐멀케이스(camelCase)를 사용하는 것이 가장 편리합니다.

    ```js
    // bad
    {
      'bermuda-triangle': {
        display: 'none',
      },
    }

    // good
    {
      bermudaTriangle: {
        display: 'none',
      },
    }
    ```

  - Use an underscore for modifiers to other styles.
  - 다른 스타일 제어자에는 언더스코어를 사용하세요. 

    > Why? Similar to BEM, this naming convention makes it clear that the styles are intended to modify the element preceded by the underscore. Underscores do not need to be quoted, so they are preferred over other characters, such as dashes.

    > 왜? BEM과 비슷하게, 이 명명규칙은 스타일이 언더스코어 앞에 있는 요소를 수정하기 위한 것임을 분명하게 해줍니다. 언더스코어는 인용될 필요가 없으므로 대시와 같은 다른 문자들보다 더 낫습니다. 

    ```js
    // bad
    {
      bruceBanner: {
        color: 'pink',
        transition: 'color 10s',
      },

      bruceBannerTheHulk: {
        color: 'green',
      },
    }

    // good
    {
      bruceBanner: {
        color: 'pink',
        transition: 'color 10s',
      },

      bruceBanner_theHulk: {
        color: 'green',
      },
    }
    ```

  - Use `selectorName_fallback` for sets of fallback styles.
  - fallback 스타일의 묶음에 `selectorName_fallback`을 사용하세요.

    > Why? Similar to modifiers, keeping the naming consistent helps reveal the relationship of these styles to the styles that override them in more adequate browsers.

    > 왜? 제어자와 비슷하게, 일관적인 명명규칙은 브라우저에서 스타일들을 오버라이드하는 스타일과의 관계를 보여주는 데 도움이 됩니다.

    ```js
    // bad
    {
      muscles: {
        display: 'flex',
      },

      muscles_sadBears: {
        width: '100%',
      },
    }

    // good
    {
      muscles: {
        display: 'flex',
      },

      muscles_fallback: {
        width: '100%',
      },
    }
    ```

  - Use a separate selector for sets of fallback styles.
  - fallback 스타일의 묶음에는 선택자 분리해서 사용하세요.

    > Why? Keeping fallback styles contained in a separate object clarifies their purpose, which improves readability.

    > 왜? 분리된 객체를 포함하는 fallback 스타일을 유지하면 이들의 목적을 분명히 할 수 있고, 가독성을 높일수도 있습니다.

    ```js
    // bad
    {
      muscles: {
        display: 'flex',
      },

      left: {
        flexGrow: 1,
        display: 'inline-block',
      },

      right: {
        display: 'inline-block',
      },
    }

    // good
    {
      muscles: {
        display: 'flex',
      },

      left: {
        flexGrow: 1,
      },

      left_fallback: {
        display: 'inline-block',
      },

      right_fallback: {
        display: 'inline-block',
      },
    }
    ```

  - Use device-agnostic names (e.g. "small", "medium", and "large") to name media query breakpoints.
  - 미디어 쿼리 브레이크 포인트에는 device-agnostic 이름 (e.g. "small", "medium", and "large")을 사용하세요.

    > Why? Commonly used names like "phone", "tablet", and "desktop" do not match the characteristics of the devices in the real world. Using these names sets the wrong expectations.

    > 왜? 일반적으로 사용되는 "phone", "tablet", "desktop"과 같은 이름은 실제 세계의 장치 특성과 일치하지 않습니다. 이러한 이름은 잘못된 예외를 만듭니다. 

    ```js
    // bad
    const breakpoints = {
      mobile: '@media (max-width: 639px)',
      tablet: '@media (max-width: 1047px)',
      desktop: '@media (min-width: 1048px)',
    };

    // good
    const breakpoints = {
      small: '@media (max-width: 639px)',
      medium: '@media (max-width: 1047px)',
      large: '@media (min-width: 1048px)',
    };
    ```

## 순서 (Ordering)

  - Define styles after the component.
  - 컴포넌트 다음에 스타일을 정의하세요.

    > Why? We use a higher-order component to theme our styles, which is naturally used after the component definition. Passing the styles object directly to this function reduces indirection.

    > 왜? 우리는 컴포넌트를 스타일의 상위에 두는데, 이는 구성 요소의 정의 이후에 자연스럽게 사용됩니다. 스타일 객체를 직접 함수에 전달하면 간접적인 참조를 줄일 수 있습니다.

    ```jsx
    // bad
    const styles = {
      container: {
        display: 'inline-block',
      },
    };

    function MyComponent({ styles }) {
      return (
        <div {...css(styles.container)}>
          Never doubt that a small group of thoughtful, committed citizens can
          change the world. Indeed, it’s the only thing that ever has.
        </div>
      );
    }

    export default withStyles(() => styles)(MyComponent);

    // good
    function MyComponent({ styles }) {
      return (
        <div {...css(styles.container)}>
          Never doubt that a small group of thoughtful, committed citizens can
          change the world. Indeed, it’s the only thing that ever has.
        </div>
      );
    }

    export default withStyles(() => ({
      container: {
        display: 'inline-block',
      },
    }))(MyComponent);
    ```

## 중첩 (Nesting)

  - Leave a blank line between adjacent blocks at the same indentation level.
  - 같은 들여쓰기 레벨에서 블록과 블록 사이에 빈 행을 넣으세요.

    > Why? The whitespace improves readability and reduces the likelihood of merge conflicts.

    > 왜? 공백은 가독성을 높이고, 병합 충돌(merge conflicts) 가능성을 줄입니다.

    ```js
    // bad
    {
      bigBang: {
        display: 'inline-block',
        '::before': {
          content: "''",
        },
      },
      universe: {
        border: 'none',
      },
    }

    // good
    {
      bigBang: {
        display: 'inline-block',

        '::before': {
          content: "''",
        },
      },

      universe: {
        border: 'none',
      },
    }
    ```

## 인라인 (Inline)

  - Use inline styles for styles that have a high cardinality (e.g. uses the value of a prop) and not for styles that have a low cardinality.
  - 카디널리티가 높은 스타일 (e.g. prop의 값을 사용)에는 인라인 스타일을 사용하고, 카디널리티가 낮은 스타일에는 사용하지 마세요.

    > Why? Generating themed stylesheets can be expensive, so they are best for discrete sets of styles.

    > 왜? 테마 스타일시트를 만드는 것은 비용이 많이 들 수 있기 때문에, 개별 스타일 시트에 가장 적합합니다.

    ```jsx
    // bad
    export default function MyComponent({ spacing }) {
      return (
        <div style={{ display: 'table', margin: spacing }} />
      );
    }

    // good
    function MyComponent({ styles, spacing }) {
      return (
        <div {...css(styles.periodic, { margin: spacing })} />
      );
    }
    export default withStyles(() => ({
      periodic: {
        display: 'table',
      },
    }))(MyComponent);
    ```

## 테마 (Themes)

  - Use an abstraction layer such as [react-with-styles](https://github.com/airbnb/react-with-styles) that enables theming. *react-with-styles gives us things like `withStyles()`, `ThemedStyleSheet`, and `css()` which are used in some of the examples in this document.*
  - 테마를 설정할 수 있게 해주는 [react-with-styles](https://github.com/airbnb/react-with-styles)와 같은 추상 레이어를 사용하세요. *react-with-styles은 `withStyles()`, `ThemedStyleSheet`, `css()`와 같이 이 문서의 예제에 사용되는 것들을 제공합니다.*

  > Why? It is useful to have a set of shared variables for styling your components. Using an abstraction layer makes this more convenient. Additionally, this can help prevent your components from being tightly coupled to any particular underlying implementation, which gives you more freedom.

  > 왜? 컴포넌트를 스타일링하기 위해 공유되는 변수들의 묶음을 사용하면 많은 도움이 되며, 추상 레이어를 사용하면 더 편리해집니다. 추가로, 이를 통해 컴포넌트가 특정 underlying implementation와 결합되지 않도록 방지할 수 있으며, 이는 더 많은 자유를 누릴 수 있게 해줍니다.

  - Define colors only in themes.
  - 테마에서만 색깔을 정의하세요.

    ```js
    // bad
    export default withStyles(() => ({
      chuckNorris: {
        color: '#bada55',
      },
    }))(MyComponent);

    // good
    export default withStyles(({ color }) => ({
      chuckNorris: {
        color: color.badass,
      },
    }))(MyComponent);
    ```

  - Define fonts only in themes.
  - 테마에서만 폰트를 정의하세요.

    ```js
    // bad
    export default withStyles(() => ({
      towerOfPisa: {
        fontStyle: 'italic',
      },
    }))(MyComponent);

    // good
    export default withStyles(({ font }) => ({
      towerOfPisa: {
        fontStyle: font.italic,
      },
    }))(MyComponent);
    ```

  - Define fonts as sets of related styles.
  - 관련 스타일의 묶음으로 폰트를 정의하세요.

    ```js
    // bad
    export default withStyles(() => ({
      towerOfPisa: {
        fontFamily: 'Italiana, "Times New Roman", serif',
        fontSize: '2em',
        fontStyle: 'italic',
        lineHeight: 1.5,
      },
    }))(MyComponent);

    // good
    export default withStyles(({ font }) => ({
      towerOfPisa: {
        ...font.italian,
      },
    }))(MyComponent);
    ```

  - Define base grid units in theme (either as a value or a function that takes a multiplier).
  - 테마에서 그리드를 정의하세요. (배수를 취하는 함수나 값으로)

    ```js
    // bad
    export default withStyles(() => ({
      rip: {
        bottom: '-6912px', // 6 feet
      },
    }))(MyComponent);

    // good
    export default withStyles(({ units }) => ({
      rip: {
        bottom: units(864), // 6 feet, assuming our unit is 8px
      },
    }))(MyComponent);

    // good
    export default withStyles(({ unit }) => ({
      rip: {
        bottom: 864 * unit, // 6 feet, assuming our unit is 8px
      },
    }))(MyComponent);
    ```

  - Define media queries only in themes.
  - 테마에서만 미디어 쿼리를 정의하세요.

    ```js
    // bad
    export default withStyles(() => ({
      container: {
        width: '100%',

        '@media (max-width: 1047px)': {
          width: '50%',
        },
      },
    }))(MyComponent);

    // good
    export default withStyles(({ breakpoint }) => ({
      container: {
        width: '100%',

        [breakpoint.medium]: {
          width: '50%',
        },
      },
    }))(MyComponent);
    ```

  - Define tricky fallback properties in themes.
  - 테마에서 tricky fallback 속성을 정의하세요.

    > Why? Many CSS-in-JavaScript implementations merge style objects together which makes specifying fallbacks for the same property (e.g. `display`) a little tricky. To keep the approach unified, put these fallbacks in the theme.

    > 왜? 많은 CSS-in-JavaScript 구현은 동일한 속성에 대한 fallback을 지정하는 스타일 객체를 함께 병합합니다. (e.g. `display`) 접근 방식을 통합적으로 유지하려면, 이러한 fallback을 테마에 두세요.

    ```js
    // bad
    export default withStyles(() => ({
      .muscles {
        display: 'flex',
      },

      .muscles_fallback {
        'display ': 'table',
      },
    }))(MyComponent);

    // good
    export default withStyles(({ fallbacks }) => ({
      .muscles {
        display: 'flex',
      },

      .muscles_fallback {
        [fallbacks.display]: 'table',
      },
    }))(MyComponent);

    // good
    export default withStyles(({ fallback }) => ({
      .muscles {
        display: 'flex',
      },

      .muscles_fallback {
        [fallback('display')]: 'table',
      },
    }))(MyComponent);
    ```

  - Create as few custom themes as possible. Many applications may only have one theme.
  - 가능한 적은 커스텀 테마를 만드세요. 많은 어플리케이션이 오직 한가지의 테마만을 가지고 있습니다.

  - Namespace custom theme settings under a nested object with a unique and descriptive key.
  - 네임스페이스 커스텀 테마 설정은 고유하고 설명적인 키와 함께 중첩된 객체에 안에 두세요.

    ```js
    // bad
    ThemedStyleSheet.registerTheme('mySection', {
      mySectionPrimaryColor: 'green',
    });

    // good
    ThemedStyleSheet.registerTheme('mySection', {
      mySection: {
        primaryColor: 'green',
      },
    });
    ```

---

CSS puns adapted from [Saijo George](https://saijogeorge.com/css-puns/).
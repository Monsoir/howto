# 如何用 TypeScript 创建一个 React 组件

## 纯 React 组件

```ts
import * as React from 'react';

// 定义组件 props
interface IComponentProps {
  aProp: number;
  bProp: string;
  // ...
}

// 定义组件 state
interface IComponentStates {
  aState: number;
  bState: string;
  // ...
}

// 定义组件
class Component extends React.Component<IComponentProps, IComponentStates> {

  // 定义组件默认 props
  static defaultProps: Partial<IComponentProps> = {
    aProp: 0,
    bProp: 'b',
  };

  constructor(props: IComponentProps) {
    super(props);
    this.state = {
      // ...
    };
  }
  
  render() {
    return (
      // ...
    );
  }
}

export default Component;

const styles = {
  aStyle: {
    // ...
  } as React.CSSProperties,
  bStyle: {
    // ...
  } as React.CSSProperties,
};
```

## 包含 React Router

```ts
import * as React from 'react';
import { RouteComponentProps } from 'react-router-dom';

// 定义组件路由参数，取参数时，this.props.match.params.xxx
interface IComponentRouteParams {
  aRouteParam: number;
  bRouteParam: string;
}

// 定义组件 props, props 扩展路由参数
interface IComponentProps extends RouteComponentProps<IComponentRouteParams> {
  aProp: number;
  bProp: string;
  // ...
}

// 定义组件 state
interface IComponentStates {
  aState: number;
  bState: string;
  // ...
}

// 定义组件
class Component extends React.Component<IComponentProps, IComponentStates> {

  // 定义组件默认 props
  static defaultProps: Partial<IComponentProps> = {
    aProp: 0,
    bProp: 'b',
  };

  constructor(props: IComponentProps) {
    super(props);
    this.state = {
      // ...
    };
  }
  
  render() {
    return (
      // ...
    );
  }
}

export default Component;

const styles = {
  aStyle: {
    // ...
  } as React.CSSProperties,
  bStyle: {
    // ...
  } as React.CSSProperties,
};
```


# 如何创建一个 React Native 组件

## 纯 React Native 组件

```js
import React, { Component } from 'react';
import PropTypes from 'prop-types';
import {
  StyleSheet,
  View,
} from 'react-native';

class AComponent extends Component {
  constructor(props) {
    super(props);
    this.state = {
      // ...
    };
  }
  
  render() {
    return (
      <View>
        { // ... }
      </View>
    );
  }
}

AComponent.propTypes = {
  aProp: PropTypes.number;
  bProp: PropTypes.string;
};

AComponent.defaultProps = {
  aProp: 0,
  bProp: 'b',
};

export default AComponent;

const styles = StyleSheet.create({
  // ...
});
```

## 包含 React-Navigation

```js
import React, { Component } from 'react';
import {
  StyleSheet,
  View,
} from 'react-native';

class AComponent extends Component {

  // 设置默认的导航配置
  static navigationOptions = ({ navigation }) => ({
    title: navigation.state.params.title, // 设置导航栏标题
    headerBackTitle: null, // 设置下一跳转页面返回按钮的标题
    // ... 还有其他很多属性
  });

  constructor(props) {
    super(props);
    this.state = {
      // ...
    };
  }
  
  render() {
    return (
      <View>
        { // ... }
      </View>
    );
  }
}

AComponent.propTypes = {
  aProp: PropTypes.number;
  bProp: PropTypes.string;
};

AComponent.defaultProps = {
  aProp: 0,
  bProp: 'b',
};

export default AComponent;

const styles = StyleSheet.create({
  // ...
});
```


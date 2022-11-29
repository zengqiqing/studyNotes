rn弹窗

关于rn组件中的弹窗问题

1.涉及到的组件

**backdrop背景板-------ok**

需支持功能：

1.背景板

2.中间内容

注意：

该组件是十分基础的组件，支持的面很多，所以需要处理得灵活一点

```Markdown
注意点：

有注意到非rn端里面有个view标签中有onTouchMove={preventDefault}，这个属性的作用是为了阻止遮罩层的穿透问题。

所以在开发rn时，请注意遮罩层穿透问题。

      
```

![img](https://xingyun.feishu.cn/space/api/box/stream/download/asynccode/?code=YWY3ZGM2NmEwNzVmZGQ3MzJlMjEwOWQ3ODM4MmNkMGJfRXUyREpsckgzaDBKSncycE9uaFFuYmlxdWpDZmlaUU5fVG9rZW46Ym94Y25nNmFpOU51VTlFcEJvRXN0MzlodDRiXzE2NTI2NzE1ODk6MTY1MjY3NTE4OV9WNA)



**popup弹窗**

支持能力：

1.多方位展示弹窗

2.关闭功能，关闭动画

3.圆角功能

显示遮盖：用的就是backdrop中的展示弹窗

其余展示是从四方位展示出来



![img](https://xingyun.feishu.cn/space/api/box/stream/download/asynccode/?code=YzllY2E0MzMzNzhkNjIyYzE4OGYzOTNmOWU2NWFkZWZfMTcyNmVub2dyZmNFb3NGY0hJZWdqek1qa0haRWY1YnJfVG9rZW46Ym94Y25kdWFrQ2NwekVtMjluMXZtZU1keHdnXzE2NTI2NzE1ODk6MTY1MjY3NTE4OV9WNA)

方案代码：

```TypeScript
import {

    Modal,

    View,

    TouchableOpacity,

    ScrollView,

    StyleSheet,

    Animated,

    Dimensions,

    Text

  } from "react-native";

  

  import { useState, forwardRef, useImperativeHandle } from "react";

  import Backdrop from "../Backdrop";

  

  const { width, height } = Dimensions.get("window");

  

  const ActionSheetRn = (props: any, ref: any) => {

    const [visible, setVisible] = useState(false);

    const [translateY] = useState(height*0.5);

    const [sheetAnim] = useState(new Animated.Value(translateY));

    useImperativeHandle(ref, () => ({

      show:show,

      hide: hide

    }));

  

    const show=  () => {

      setVisible(true);

      Animated.timing(sheetAnim, {

        toValue: 0,

        duration: 250

      }).start();

    }

  

    const hide = () => {

      Animated.timing(sheetAnim, {

        toValue: height - 310,

        duration: 250

      }).start(() => {

        setVisible(false);

      });

    };

  

    const _renderTitle = () => {

      return <Text style={[styles.titleText]}>标题2222222222</Text>;

    };

  

    const _renderContainer = () => {

      return (

        <View>

          <Text>我是内容咯！！！！</Text>

        </View>

      );

    };

  console.log(sheetAnim,'sheetAnims')

    return (

      <View>

        <View>

          <Text onPress={()=>{show()}}>点击打开</Text>

        </View>

        <Backdrop open={visible}>

  

      <Modal

        visible={visible}

        transparent

        animationType="none"

        onRequestClose={hide}

      >

        <View style={styles.wrapper}>

          <TouchableOpacity style={styles.overlay} onPress={hide}>

            <Animated.View

              style={[

                styles.content_container,

                {

                  height: translateY,

                //   transform: [{ translateY: sheetAnim }]

                transform: [{ translateX: sheetAnim }]

                }

              ]}

            >

              {_renderTitle()}

              <ScrollView horizontal showsHorizontalScrollIndicator={false}>

                {_renderContainer()}

              </ScrollView>

            </Animated.View>

          </TouchableOpacity>

        </View>

      </Modal>

        </Backdrop>

  

      </View>

    );

  };

  

  const styles = StyleSheet.create({

      wrapper: {

        flex: 1,

        width: width,

        alignItems: "center",

        justifyContent: "flex-end",

        backgroundColor: 1

        // backgroundColor: "rgba(0,0,0,0.5)"

      },

      content_container: {

        width: width,

        backgroundColor: "#fff",

        position: 'relative',

        // position: 'absolute',

        left:0,

        bottom:0

      },

    });

    

  export default forwardRef(ActionSheetRn);
```

方案二：借鉴组件库代码实现左右两侧弹出

https://github.com/pedreviljoen/react-native-menu/blob/master/index.js

左右两侧弹窗

```TypeScript
import React, { useState, useEffect, useRef,CSSProperties } from "react";

import {

  Animated,

  useWindowDimensions,

  StyleSheet,

  View,

  Text,

  TouchableOpacity

} from "react-native";



interface MenuDrawer {

  drawerWithPercentage?: number;

  drawerHeightPercentage?: number;

  open: boolean;

  position?: string;

  animationTime?: number;

  opacity?: number;

  onClose: () => void;

  containerHeight?: number;

  drawerOverlayStyle?:string | CSSProperties

}

const MenuDrawer = props => {

  const fadeAnim = useRef(new Animated.Value(0)).current;

  const window = useWindowDimensions();

  const screenHeight = window.height;

  const screenWidth = window.width;



  const initialDrawerWidth = screenWidth * (props.drawerWithPercentage / 100);

  const initialDrawerHeight = screenWidth * (props.drawerHeightPercentage / 100);



  const drawerWidthRef = useRef(initialDrawerWidth);



  const leftOffsetRef = useRef(new Animated.Value(0));



  const backdropFadeAnim = useRef(new Animated.Value(0)).current; // 透明度初始值设为0

  const [backDrop, setBackDrop] = useState<boolean>(false);



  useEffect(() => {

    const newDrawerWidth = screenWidth * (props.drawerWithPercentage / 100);

    drawerWidthRef.current = newDrawerWidth;

    if (props.open) {

      leftOffsetRef.current = new Animated.Value(

        determineNewLeftOffset(drawerWidthRef.current)

      );

    } else {

      leftOffsetRef.current = new Animated.Value(0);

    }

  }, [screenWidth]);



  const determineNewLeftOffset = drawerWidth => {

    return props.position === "left" ? drawerWidth : -drawerWidth;

  };

  useEffect(() => {

    props.open ? openDrawer() : closeDrawer();

  }, [props.open]);



  const openDrawer = () => {

    const { drawerWithPercentage, animationTime, opacity } = props;

    const drawerWidth = drawerWidthRef.current;

    const leftOffset = leftOffsetRef.current;

    setBackDrop(true);



    Animated.parallel([

      Animated.timing(leftOffset, {

        toValue: determineNewLeftOffset(drawerWidth),

        duration: animationTime,

        useNativeDriver: true

      }),

      Animated.timing(fadeAnim, {

        toValue: opacity,

        duration: animationTime,

        useNativeDriver: true

      })

    ]).start();



    Animated.timing(

      // 随时间变化而执行动画

      backdropFadeAnim, // 动画中的变量值

      {

        toValue:

         1, // 透明度最终变为1，即完全不透明

        duration: 1000 // 让动画持续一段时间

      }

    ).start(); // 开始执行动画

  };



  const closeDrawer = () => {

    const { drawerWithPercentage, animationTime } = props;

    const drawerWidth = drawerWidthRef.current;

    const leftOffset = leftOffsetRef.current;



    Animated.parallel([

      Animated.timing(leftOffset, {

        toValue: 0,

        duration: animationTime,

        useNativeDriver: true

      }),

      Animated.timing(fadeAnim, {

        toValue: 1,

        duration: animationTime,

        useNativeDriver: true

      })

    ]).start(res => {

      setBackDrop(false);

    });

  };



  const drawerFallback = () => {

    return (

      <TouchableOpacity onPress={props.onClose}>

        <Text>关闭</Text>

      </TouchableOpacity>

    );

  };



  const renderOverlay = () => {

    const { children, drawerHeightPercentage,position,containerHeight,drawerOverlayStyle, drawerContent } = props;

    const drawerWidth = drawerWidthRef.current;

    const leftOffset = leftOffsetRef.current;

    const animated = { transform: [{ translateX: leftOffset }] };

    return (

      <>

        <Animated.View

          style={[

            animated,

            styles.drawerOverlay,

            {

              height: drawerHeightPercentage?initialDrawerHeight:screenHeight,

              width: drawerWidth,

              [position]: -drawerWidth,

              ...drawerOverlayStyle

            }

          ]}

        >

          <View>

            {drawerContent ? drawerContent() : drawerFallback()}

            {children}

            <TouchableOpacity onPress={props.onClose}>

              <Text>关闭按钮</Text>

            </TouchableOpacity>

          </View>

        </Animated.View>

        {backDrop ? (

          <TouchableOpacity activeOpacity={1} onPress={props.onClose}

            style={[styles.mask,{

              height:screenHeight,

            }]}

          ></TouchableOpacity>

        ) : null}

      </>

    );

  };



  return renderOverlay();

};



const styles = StyleSheet.create({

  containerOverlay: {

    flex: 1,

    elevation: 0, // better for android

    zIndex: 0,

    backgroundColor: "purple"

  },



  drawerOverlay: {

    position: "absolute",

    backgroundColor: "pink",

    elevation: 2, // better for android

    zIndex: 2,

  },

  mask:{

    zIndex: 1,

    width: '100%',

    position: "absolute",

    right: 0,

    top: 0,

    backgroundColor: "rgba(0, 0, 0, 0.4)",

  }

});



export default MenuDrawer;















//调用

import {

  Modal,

  View,

  TouchableOpacity,

  ScrollView,

  StyleSheet,

  Animated,

  Dimensions,

  Text,

  Button

} from "react-native";

import Demo from "./demo";

import { useState, forwardRef } from "react";



const ActionSheetRn = (props: any, ref: any) => {

  const [visible, setVisible] = useState(false);



  const drawerContent = () => {

    return (

      <TouchableOpacity onPress={() => setVisible(false)}>

        <Text>关闭按钮</Text>

      </TouchableOpacity>

    );

  };

  const DemoView = ()=>{

    return (

      <View>

          <Text>该组件是十分基础的组件，支持的面很多，所以需要处理得灵活一点该组件是十分基础的组件，支持的面很多，所以需要处理得灵活一点该组件是十分基础的组件，支持的面很多

          该组件是十分基础的组件，支持的面很多，所以需要处理得灵活一点，所以需要处理得灵活一点该组件是十分基础的组件，支持的面很多，所以需要处理得灵活一点该组件是十分基础的组件，支持的面很多，所以需要处理得灵活一点</Text>

        </View>

    )

  }

  return (

    <View>

      {/*<TouchableOpacity onPress={() => setVisible(true)}>

        <Text>Open</Text>

      </TouchableOpacity>

      <Demo

        open={visible}

        position="left"

        drawerContent={drawerContent}

        drawerPercentage={45}

        containerHeight={400}

        animationTime={250}

        onClose={() => setVisible(false)}

        overlay

        drawerOverlayStyle={{top:0}}

        opacity={0.4}

      ></Demo> */}

     

      <Demo

        open={visible}

        position="right"

        drawerContent={drawerContent}

        drawerWithPercentage={45}

        drawerHeightPercentage={45}



        animationTime={250}

        onClose={() => setVisible(false)}

        // drawerOverlayStyle={{ top: 100 }}

        opacity={0.1}

      >

        <DemoView />

      </Demo>

      <View style={{ backgroundColor: "red" }}>

      <Button onPress={() => setVisible(true)} title="翻牌"></Button>

      </View>

    </View>

  );

};



export default forwardRef(ActionSheetRn);
```

下弹窗：第一版

```TypeScript
import { useState, useEffect, useRef,CSSProperties } from "react";

import {

  Animated,

  useWindowDimensions,

  StyleSheet,

  View,

  Text,

  TouchableOpacity

} from "react-native";



interface MenuDrawer {

  drawerWithPercentage?: number;

  drawerHeightPercentage?: number;

  open: boolean;

  position?: string;

  animationTime?: number;

  opacity?: number;

  onClose: () => void;

  containerHeight?: number;

  drawerOverlayStyle?:string | CSSProperties

}

const MenuDrawer = props => {

  const fadeAnim = useRef(new Animated.Value(0)).current;

  const window = useWindowDimensions();

  const screenHeight = window.height;

  const screenWidth = window.width;



  const initialDrawerHeight = screenHeight * (props.drawerHeightPercentage / 100);



  const drawerHeightRef = useRef(initialDrawerHeight);

//   偏移值

  const leftOffsetRef = useRef(new Animated.Value(0));

  const [backDrop, setBackDrop] = useState<boolean>(false);



  useEffect(() => {

    const newDrawerHeight = screenHeight * (props.drawerHeightPercentage / 100);

    drawerHeightRef.current = newDrawerHeight;

    if (props.open) {

      leftOffsetRef.current = new Animated.Value(

        determineNewLeftOffset(drawerHeightRef.current)

      );

    } else {

      leftOffsetRef.current = new Animated.Value(0);

    }

  }, [screenWidth]);



  const determineNewLeftOffset = drawerHeight => {

    return props.position === "top" ? drawerHeight : -drawerHeight;

  };

  useEffect(() => {

    props.open ? openDrawer() : closeDrawer();

  }, [props.open]);



  const openDrawer = () => {

    const {  animationTime, opacity } = props;

    const drawerHeight = drawerHeightRef.current;

    console.log(drawerHeight,'-- drawerHeight --')

    const leftOffset = leftOffsetRef.current;

    setBackDrop(true);



    Animated.parallel([

      Animated.timing(leftOffset, {

        toValue: determineNewLeftOffset(drawerHeight),

        duration: animationTime,

        useNativeDriver: true

      }),

      Animated.timing(fadeAnim, {

        toValue: opacity,

        duration: animationTime,

        useNativeDriver: true

      })

    ]).start();



  };



  const closeDrawer = () => {

    const { animationTime } = props;

    const leftOffset = leftOffsetRef.current;



    Animated.parallel([

      Animated.timing(leftOffset, {

        toValue: 0,

        duration: animationTime,

        useNativeDriver: true

      }),

      Animated.timing(fadeAnim, {

        toValue: 1,

        duration: animationTime,

        useNativeDriver: true

      })

    ]).start(res => {

      setBackDrop(false);

    });

  };



  const drawerFallback = () => {

    return (

      <TouchableOpacity onPress={props.onClose}>

        <Text>关闭</Text>

      </TouchableOpacity>

    );

  };



  const renderOverlay = () => {

    const { children, drawerHeightPercentage,position,drawerOverlayStyle, drawerContent } = props;

    const drawerWidth = drawerHeightRef.current;

    const leftOffset = leftOffsetRef.current;

    const animated = { transform: [{ translateY: leftOffset }] };

    return (

      <>

        <Animated.View

          style={[

            animated,

            styles.drawerOverlay,

            {

              height: drawerHeightPercentage?initialDrawerHeight:screenHeight,

              width: screenWidth,

            [position]: -(initialDrawerHeight+screenHeight),

              ...drawerOverlayStyle

            }

          ]}

        >

            {/* 关闭按钮 */}

          <View>

            {drawerContent ? drawerContent() : drawerFallback()}

            {children}

            <TouchableOpacity onPress={props.onClose}>

              <Text>关闭按钮</Text>

            </TouchableOpacity>

          </View>

        </Animated.View>

        {/* 遮罩层 */}

        {backDrop ? (

          <TouchableOpacity activeOpacity={1} onPress={props.onClose}

            style={[styles.mask,{

              height:screenHeight,

            }]}

          ></TouchableOpacity>

        ) : null}

      </>

    );

  };



  return renderOverlay();

};



const styles = StyleSheet.create({

  containerOverlay: {

    flex: 1,

    elevation: 0, // better for android

    zIndex: 0,

    backgroundColor: "purple"

  },



  drawerOverlay: {

    position: "absolute",

    backgroundColor: "pink",

    elevation: 2, // better for android

    zIndex: 2,

  },

  mask:{

    zIndex: 1,

    width: '100%',

    position: "absolute",

    right: 0,

    top: 0,

    backgroundColor: "rgba(0, 0, 0, 0.4)",

  }

});



export default MenuDrawer;
```

下弹窗使用modal封装，但没法实现缓慢关闭弹窗功能

```TypeScript
import {

  useState,

  useEffect,

  useRef,

  Children,

  CSSProperties,

  ReactNode,

  ReactElement,

  isValidElement

} from "react";

import * as _ from "lodash";

import * as React from "react";

import {

  Animated,

  useWindowDimensions,

  StyleSheet,

  View,

  Text,

  TouchableOpacity,

  NativeModules,

  Dimensions,

  Modal

} from "react-native";



const { StatusBarManager } = NativeModules;



import { ITouchEvent } from "@tarojs/components/types/common";

import classNames from "classnames";

import { prefixClassname } from "../../styles";

import PopupContext from "./popup.context";

import PopupBackdrop from "./popup-backdrop";

import PopupClose from "./popup-close";

import { PopupPlacement, PopupPlacementString } from "./popup.shared";

import "./popup.scss";



interface PopupChildren {

  backdrop?: ReactNode;

  close?: ReactNode;

  content: ReactNode[];

}

function findPopupChildren(node?: ReactNode): PopupChildren {

  const children: PopupChildren = {

    backdrop: undefined,

    close: undefined,

    content: []

  };

  Children.forEach(node, (child: ReactNode) => {

    if (isValidElement(child)) {

      const element = child as ReactElement;

      let BackdropStr = PopupBackdrop.toString();

      let PopupCloseStr = PopupClose.toString();

      let elementStr = element.type.toString();

      if (elementStr === BackdropStr) {

        children.backdrop = element;

      } else if (elementStr === PopupCloseStr) {

        children.close = element;

      } else {

        children.content.push(child);

      }

    } else {

      children.content.push(child);

    }

  });



  return children;

}



export interface PopupProps {

  className?: string;

  style?: CSSProperties;

  open: boolean;

  duration?: number;

  rounded?: boolean;

  children?: ReactNode;

  placement?: PopupPlacement | PopupPlacementString;

  onOpen?: (opened: boolean) => void;

  onClose?: (opened: boolean) => void;

  onClosed?: () => void;



  drawerWithPercentage?: number;

  drawerHeightPercentage?: number;

  opacity?: number;

  onClick: (event: ITouchEvent) => void;

  containerHeight?: number;

}

const Popup = (props: PopupProps) => {

  const window = useWindowDimensions();

  const screenHeight = window.height;

  const screenWidth = window.width;

  const {

    className,

    style,

    open,

    placement = "left", //默认为左弹窗

    rounded = false,

    drawerWithPercentage = 100,

    drawerHeightPercentage = 100,

    children,

    onClose,

    onOpen,

    duration = 300

  } = props;



  const { backdrop = <PopupBackdrop />, close, content } = findPopupChildren(

    children

  );

  const initialDrawerWidth = screenWidth * (drawerWithPercentage / 100);

  const initialDrawerHeight = screenHeight * (drawerHeightPercentage / 100);

  // 偏移值

  const leftOffsetRef = useRef(new Animated.Value(0));

  //  背景

  const [upDownDirection] = useState(["top", "bottom"].includes(placement)); //是否为上下方向

  const drawerRef = useRef(

    upDownDirection ? initialDrawerHeight : initialDrawerWidth

  );



  useEffect(() => {

    drawerRef.current = upDownDirection

      ? initialDrawerHeight

      : initialDrawerWidth;

    if (open) {

      leftOffsetRef.current = new Animated.Value(

        determineNewLeftOffset(drawerRef.current)

      );

    } else {

      leftOffsetRef.current = new Animated.Value(0);

    }

  }, [screenWidth]);



  const determineNewLeftOffset = drawerHeight => {

    return ["top", "left"].includes(placement) ? drawerHeight : -drawerHeight;

  };

  useEffect(() => {

    open ? openDrawer() : closeDrawer();

  }, [open]);



  const openDrawer = () => {

    const drawerWidthOrHeight = drawerRef.current;

    const leftOffset = leftOffsetRef.current;

    onOpen?.(true);

    Animated.timing(leftOffset, {

      toValue: determineNewLeftOffset(drawerWidthOrHeight),

      duration: duration,

      useNativeDriver: true

    }).start();

  };



  const closeDrawer = () => {

    const leftOffset = leftOffsetRef.current;

    Animated.timing(leftOffset, {

      toValue: 0,

      duration: duration,

      useNativeDriver: true

    }).start();

  };



  const drawerWidth = drawerRef.current;

  const leftOffset = leftOffsetRef.current;



  const configAggregate = {

    top: {

      width: screenWidth,

      positionValue: -drawerWidth,

      animated: { transform: [{ translateY: leftOffset }] }

    },

    bottom: {

      width: screenWidth,

      positionValue: -(initialDrawerHeight + screenHeight),

      animated: { transform: [{ translateY: leftOffset }] }

    },

    left: {

      width: drawerWidth,

      positionValue: -drawerWidth,

      animated: { transform: [{ translateX: leftOffset }] }

    },

    right: {

      width: drawerWidth,

      positionValue: -drawerWidth,

      animated: { transform: [{ translateX: leftOffset }] }

    }

  };

  const width = placement ? configAggregate[placement].width : 0;

  const positionValue = placement

    ? configAggregate[placement].positionValue

    : 0;

  const animated = placement ? configAggregate[placement].animated : {};

  console.log(open, "open");



  return (

    <PopupContext.Provider

      value={{

        open,

        placement,

        emitClose: onClose

      }}

    >

      {backdrop}

      {placement === "bottom" ? (

        <Modal

          visible={open}

          transparent

          animationType="slide"

        >

          <View

            style={{

              flex: 1,

              justifyContent: "flex-end",

              alignItems: "center"

            }}

          >

            <View

              className={classNames(

                prefixClassname("popup"),

                {

                  [prefixClassname("popup--toprounded")]:

                    placement === PopupPlacement.Top && rounded,

                  [prefixClassname("popup--bottomrounded")]:

                    placement === PopupPlacement.Bottom && rounded,

                  [prefixClassname("popup--leftrounded")]:

                    placement === PopupPlacement.Left && rounded,

                  [prefixClassname("popup--rightrounded")]:

                    placement === PopupPlacement.Right && rounded

                },

                className

              )}

              style={{

                height: initialDrawerHeight,

                width: "100%",

                backgroundColor: "#fff",

                zIndex: 1000000

              }}

            >

              {close}

              {content}

            </View>

          </View>

        </Modal>

      ) : (

        <Animated.View

          className={classNames(

            prefixClassname("popup"),

            {

              [prefixClassname("popup--toprounded")]:

                placement === PopupPlacement.Top && rounded,

              [prefixClassname("popup--bottomrounded")]:

                placement === PopupPlacement.Bottom && rounded,

              [prefixClassname("popup--leftrounded")]:

                placement === PopupPlacement.Left && rounded,

              [prefixClassname("popup--rightrounded")]:

                placement === PopupPlacement.Right && rounded

            },

            className

          )}

          style={[

            animated,

            placement && {

              width: width,

              height: drawerHeightPercentage

                ? initialDrawerHeight

                : screenHeight,

              [placement]: positionValue

            },

            { ...style }

          ]}

        >

          {close}

          {content}

        </Animated.View>

      )}

    </PopupContext.Provider>

  );

};



const styles = StyleSheet.create({

  mask: {

    zIndex: 1,

    width: "100%",

    position: "absolute",

    right: 0,

    top: 0

  },

  defaultClose: {

    alignItems: "flex-end",

    marginRight: 10

  },

  defaultCloseText: {

    fontSize: 26,

    color: "#c8c9cc"

  }

});



export default Popup;
```

上弹窗

```TypeScript
import { useState, useEffect, useRef,CSSProperties } from "react";

import {

  Animated,

  useWindowDimensions,

  StyleSheet,

  View,

  Text,

  TouchableOpacity

} from "react-native";



interface MenuDrawer {

  drawerWithPercentage?: number;

  drawerHeightPercentage?: number;

  open: boolean;

  position?: string;

  animationTime?: number;

  opacity?: number;

  onClose: () => void;

  containerHeight?: number;

  drawerOverlayStyle?:string | CSSProperties

}

const MenuDrawer = props => {

  const fadeAnim = useRef(new Animated.Value(0)).current;

  const window = useWindowDimensions();

  const screenHeight = window.height;

  const screenWidth = window.width;



  const initialDrawerHeight = screenHeight * (props.drawerHeightPercentage / 100);



  const drawerHeightRef = useRef(initialDrawerHeight);



//   偏移值

  const leftOffsetRef = useRef(new Animated.Value(0));



  const [backDrop, setBackDrop] = useState<boolean>(false);



  useEffect(() => {

    const newDrawerHeight = screenHeight * (props.drawerHeightPercentage / 100);

    drawerHeightRef.current = newDrawerHeight;

    if (props.open) {

      leftOffsetRef.current = new Animated.Value(

        determineNewLeftOffset(drawerHeightRef.current)

      );

    } else {

      leftOffsetRef.current = new Animated.Value(0);

    }

  }, [screenWidth]);



  const determineNewLeftOffset = drawerHeight => {

    return props.position === "top" ? drawerHeight : -drawerHeight;

  };

  useEffect(() => {

    props.open ? openDrawer() : closeDrawer();

  }, [props.open]);



  const openDrawer = () => {

    const {  animationTime, opacity } = props;

    const drawerWidth = drawerHeightRef.current;

    const leftOffset = leftOffsetRef.current;

    setBackDrop(true);



    Animated.parallel([

      Animated.timing(leftOffset, {

        toValue: determineNewLeftOffset(drawerWidth),

        duration: animationTime,

        useNativeDriver: true

      }),

      Animated.timing(fadeAnim, {

        toValue: opacity,

        duration: animationTime,

        useNativeDriver: true

      })

    ]).start();



  };



  const closeDrawer = () => {

    const { animationTime } = props;

    const leftOffset = leftOffsetRef.current;



    Animated.parallel([

      Animated.timing(leftOffset, {

        toValue: 0,

        duration: animationTime,

        useNativeDriver: true

      }),

      Animated.timing(fadeAnim, {

        toValue: 1,

        duration: animationTime,

        useNativeDriver: true

      })

    ]).start(res => {

      setBackDrop(false);

    });

  };



  const drawerFallback = () => {

    return (

      <TouchableOpacity onPress={props.onClose}>

        <Text>关闭</Text>

      </TouchableOpacity>

    );

  };



  const renderOverlay = () => {

    const { children, drawerHeightPercentage,position,drawerOverlayStyle, drawerContent } = props;

    const drawerWidth = drawerHeightRef.current;

    const leftOffset = leftOffsetRef.current;

    const animated = { transform: [{ translateY: leftOffset }] };

    return (

      <>

        <Animated.View

          style={[

            animated,

            styles.drawerOverlay,

            {

              height: drawerHeightPercentage?initialDrawerHeight:screenHeight,

              width: screenWidth,

              [position]: -drawerWidth,

              ...drawerOverlayStyle

            }

          ]}

        >

          <View>

            {drawerContent ? drawerContent() : drawerFallback()}

            {children}

            <TouchableOpacity onPress={props.onClose}>

              <Text>关闭按钮</Text>

            </TouchableOpacity>

          </View>

        </Animated.View>

        {backDrop ? (

          <TouchableOpacity activeOpacity={1} onPress={props.onClose}

            style={[styles.mask,{

              height:screenHeight,

            }]}

          ></TouchableOpacity>

        ) : null}

      </>

    );

  };



  return renderOverlay();

};



const styles = StyleSheet.create({

  containerOverlay: {

    flex: 1,

    elevation: 0, // better for android

    zIndex: 0,

    backgroundColor: "purple"

  },



  drawerOverlay: {

    position: "absolute",

    backgroundColor: "pink",

    elevation: 2, // better for android

    zIndex: 2,

  },

  mask:{

    zIndex: 1,

    width: '100%',

    position: "absolute",

    right: 0,

    top: 0,

    backgroundColor: "rgba(0, 0, 0, 0.4)",

  }

});



export default MenuDrawer;
```

下弹窗在实现上面的实现过程中，发现有定位问题，所以曲线救国，bottom另起炉灶写，代码确实不是很美丽，还老觉得动画有点卡顿，后期可以再优化优化，或者重新拿上面的代码分析一下，什么原因导致他的定位参照物有问题：

```TypeScript
import {

  useState,

  useEffect,

  useRef,

  Children,

  CSSProperties,

  ReactNode,

  ReactElement,

  isValidElement

} from "react";

import * as _ from "lodash";

import * as React from "react";

import {

  Animated,

  useWindowDimensions,

  StyleSheet,

  View,

  Text,

  TouchableOpacity,

  NativeModules,

  Dimensions,

  ScrollView,

  Modal

} from "react-native";



import { ITouchEvent } from "@tarojs/components/types/common";

import classNames from "classnames";

import { prefixClassname } from "../../styles";

import PopupContext from "./popup.context";

import PopupBackdrop from "./popup-backdrop";

import PopupClose from "./popup-close";

import { PopupPlacement, PopupPlacementString } from "./popup.shared";

import "./popup.scss";



const { StatusBarManager } = NativeModules;



interface PopupChildren {

  backdrop?: ReactNode;

  close?: ReactNode;

  content: ReactNode[];

}

function findPopupChildren(node?: ReactNode): PopupChildren {

  const children: PopupChildren = {

    backdrop: undefined,

    close: undefined,

    content: []

  };

  Children.forEach(node, (child: ReactNode) => {

    if (isValidElement(child)) {

      const element = child as ReactElement;

      let BackdropStr = PopupBackdrop.toString();

      let PopupCloseStr = PopupClose.toString();

      let elementStr = element.type.toString();

      if (elementStr === BackdropStr) {

        children.backdrop = element;

      } else if (elementStr === PopupCloseStr) {

        children.close = element;

      } else {

        children.content.push(child);

      }

    } else {

      children.content.push(child);

    }

  });



  return children;

}



export interface PopupProps {

  className?: string;

  style?: CSSProperties;

  open: boolean;

  duration?: number;

  rounded?: boolean;

  children?: ReactNode;

  placement?: PopupPlacement | PopupPlacementString;

  onOpen?: (opened: boolean) => void;

  onClose?: (opened: boolean) => void;

  onClosed?: () => void;



  drawerWithPercentage?: number;

  drawerHeightPercentage?: number;

  opacity?: number;

  onClick: (event: ITouchEvent) => void;

  containerHeight?: number;

}

const Popup = (props: PopupProps) => {

  const window = useWindowDimensions();

  const screenHeight = window.height;

  const screenWidth = window.width;

  const {

    className,

    style,

    open,

    placement = "left", //默认为左弹窗

    rounded = false,

    drawerWithPercentage = 100,

    drawerHeightPercentage = 100,

    children,

    onClose,

    onOpen,

    duration = 300

  } = props;



  const { backdrop = <PopupBackdrop />, close, content } = findPopupChildren(

    children

  );

  const initialDrawerWidth = screenWidth * (drawerWithPercentage / 100);

  const initialDrawerHeight = screenHeight * (drawerHeightPercentage / 100);

  // 偏移值

  const leftOffsetRef = useRef(new Animated.Value(0));

  const sheetAnim = useRef(new Animated.Value(initialDrawerWidth)).current;



  //  背景

  const [upDownDirection] = useState(["top", "bottom"].includes(placement)); //是否为上下方向

  const drawerRef = useRef(

    upDownDirection ? initialDrawerHeight : initialDrawerWidth

  );



  const [closebottom, setClosebottom] = useState(false);

  useEffect(() => {

    drawerRef.current = upDownDirection

      ? initialDrawerHeight

      : initialDrawerWidth;

    if (open) {

      leftOffsetRef.current = new Animated.Value(

        determineNewLeftOffset(drawerRef.current)

      );

    } else {

      leftOffsetRef.current = new Animated.Value(0);

    }

  }, [screenWidth]);



  const determineNewLeftOffset = drawerHeight => {

    return ["top", "left"].includes(placement) ? drawerHeight : -drawerHeight;

  };

  useEffect(() => {

    open ? openDrawer() : closeDrawer();

    if (open && placement === PopupPlacement.Bottom) setClosebottom(true);

  }, [open, placement]);

  useEffect(() => {

    if (placement === PopupPlacement.Bottom) {

      closebottom && show();

      !open && closebottom && hide();

    }

  }, [closebottom, open, placement]);



  const show = () => {

    Animated.timing(sheetAnim, {

      toValue: 0,

      duration: duration,

      useNativeDriver: false

    }).start();

  };



  const hide = () => {

    Animated.timing(sheetAnim, {

      toValue: initialDrawerWidth,

      duration: duration,

      useNativeDriver: false

    }).start(() => {

      setClosebottom(false);

    });

  };



  const openDrawer = () => {

    const drawerWidthOrHeight = drawerRef.current;

    const leftOffset = leftOffsetRef.current;

    onOpen?.(true);

    Animated.timing(leftOffset, {

      toValue: determineNewLeftOffset(drawerWidthOrHeight),

      duration: duration,

      useNativeDriver: true

    }).start();

  };



  const closeDrawer = () => {

    const leftOffset = leftOffsetRef.current;

    Animated.timing(leftOffset, {

      toValue: 0,

      duration: duration,

      useNativeDriver: true

    }).start();

  };



  const drawerWidth = drawerRef.current;

  const leftOffset = leftOffsetRef.current;



  const configAggregate = {

    top: {

      width: screenWidth,

      positionValue: -drawerWidth,

      animated: { transform: [{ translateY: leftOffset }] }

    },

    // bottom: {

    //   width: screenWidth,

    //   positionValue: -(initialDrawerHeight + screenHeight),

    //   animated: { transform: [{ translateY: leftOffset }] }

    // },

    left: {

      width: drawerWidth,

      positionValue: -drawerWidth,

      animated: { transform: [{ translateX: leftOffset }] }

    },

    right: {

      width: drawerWidth,

      positionValue: -drawerWidth,

      animated: { transform: [{ translateX: leftOffset }] }

    }

  };

  const width = configAggregate[placement]?.width ?? 0;

  const positionValue = configAggregate[placement]?.positionValue ?? 0;

  const animated = configAggregate[placement]?.animated ?? {};

  return (

    <PopupContext.Provider

      value={{

        open,

        placement,

        emitClose: onClose

      }}

    >

      {placement === PopupPlacement.Bottom ? (

        <Modal visible={closebottom} transparent animationType="none">

          <View

            style={[

              styles.wrapper,

              {

                width: screenWidth

              }

            ]}

          >

            {backdrop}

            <Animated.View

              className={classNames(

                prefixClassname("popup"),

                {

                  [prefixClassname("popup--bottomrounded")]:

                    placement === PopupPlacement.Bottom && rounded

                },

                className

              )}

              style={[

                styles.content_container,

                {

                  width: screenWidth,

                  height: initialDrawerHeight,

                  transform: [{ translateY: sheetAnim }]

                }

              ]}

            >

              <ScrollView horizontal showsHorizontalScrollIndicator={false}>

                {content}

              </ScrollView>

              {close}

            </Animated.View>

          </View>

        </Modal>

      ) : (

        <>

          {backdrop}



          <Animated.View

            className={classNames(

              prefixClassname("popup"),

              {

                [prefixClassname("popup--toprounded")]:

                  placement === PopupPlacement.Top && rounded,

                [prefixClassname("popup--leftrounded")]:

                  placement === PopupPlacement.Left && rounded,

                [prefixClassname("popup--rightrounded")]:

                  placement === PopupPlacement.Right && rounded

              },

              className

            )}

            style={[

              animated,

              placement && {

                width: width,

                height: drawerHeightPercentage

                  ? initialDrawerHeight

                  : screenHeight,

                [placement]: positionValue

              },

              { ...style }

            ]}

          >

            {close}

            {content}

          </Animated.View>

        </>

      )}

    </PopupContext.Provider>

  );

};



const styles = StyleSheet.create({

  wrapper: {

    flex: 1,

    alignItems: "center",

    justifyContent: "flex-end"

    // backgroundColor: "rgba(0,0,0,0.5)"

  },

  content_container: {

    backgroundColor: "#fff",

    position: "relative",

    // position: 'absolute',

    left: 0,

    bottom: 0

  }

});

export default Popup;
```

以下要解决弹窗会被软键盘遮挡问题。

接入第三方：React-native-keyboard-aware-scroll-view 监听键盘弹起时，遮挡问题

https://github.com/APSL/react-native-keyboard-aware-scroll-view

下面这个版本就是把软键盘的问题解决的啦，但这个是分开了bottom和其他状态弹窗封装的。所以应该要再优化一下，不应该分开不同方向分装。但都先记录一下啦

```TypeScript
import {

  useState,

  useEffect,

  useRef,

  Children,

  CSSProperties,

  ReactNode,

  ReactElement,

  isValidElement

} from "react";

import * as _ from "lodash";

import * as React from "react";

import { Animated, useWindowDimensions, Modal } from "react-native";

import { KeyboardAwareScrollView } from "react-native-keyboard-aware-scroll-view";

import { ITouchEvent } from "@tarojs/components/types/common";

import classNames from "classnames";

import { prefixClassname } from "../../styles";

import PopupContext from "./popup.context";

import PopupBackdrop from "./popup-backdrop";

import PopupClose from "./popup-close";

import { PopupPlacement, PopupPlacementString } from "./popup.shared";

import "./popup.scss";

interface PopupChildren {

  backdrop?: ReactNode;

  close?: ReactNode;

  content: ReactNode[];

}

function findPopupChildren(node?: ReactNode): PopupChildren {

  const children: PopupChildren = {

    backdrop: undefined,

    close: undefined,

    content: []

  };

  Children.forEach(node, (child: ReactNode) => {

    if (isValidElement(child)) {

      const element = child as ReactElement;

      let BackdropStr = PopupBackdrop.toString();

      let PopupCloseStr = PopupClose.toString();

      let elementStr = element.type.toString();

      if (elementStr === BackdropStr) {

        children.backdrop = element;

      } else if (elementStr === PopupCloseStr) {

        children.close = element;

      } else {

        children.content.push(child);

      }

    } else {

      children.content.push(child);

    }

  });



  return children;

}



export interface PopupProps {

  className?: string;

  style?: CSSProperties;

  open: boolean;

  duration?: number;

  rounded?: boolean;

  children?: ReactNode;

  placement?: PopupPlacement | PopupPlacementString;

  onOpen?: (opened: boolean) => void;

  onClose?: (opened: boolean) => void;

  onClosed?: () => void;



  drawerWithPercentage?: number;

  drawerHeightPercentage?: number;

  opacity?: number;

  onClick: (event: ITouchEvent) => void;

  containerHeight?: number;

}

const Popup = (props: PopupProps) => {

  const window = useWindowDimensions();

  const screenHeight = window.height;

  const screenWidth = window.width;

  const {

    className,

    style,

    open,

    placement = "left", //默认为左弹窗

    rounded = false,

    drawerWithPercentage = 100,

    drawerHeightPercentage = 100,

    children,

    onClose,

    onOpen,

    duration = 300

  } = props;



  const { backdrop = <PopupBackdrop />, close, content } = findPopupChildren(

    children

  );

  const initialDrawerWidth = screenWidth * (drawerWithPercentage / 100);

  const initialDrawerHeight = screenHeight * (drawerHeightPercentage / 100);

  // 偏移值

  const leftOffsetRef = useRef(new Animated.Value(0));

  const sheetAnimRef = useRef(new Animated.Value(initialDrawerHeight)).current;



  //  背景

  const [upDownDirection] = useState(["top", "bottom"].includes(placement)); //是否为上下方向

  const drawerRef = useRef(

    upDownDirection ? initialDrawerHeight : initialDrawerWidth

  );



  const [closebottom, setClosebottom] = useState(false);

  useEffect(() => {

    drawerRef.current = upDownDirection

      ? initialDrawerHeight

      : initialDrawerWidth;

    if (open) {

      leftOffsetRef.current = new Animated.Value(

        determineNewLeftOffset(drawerRef.current)

      );

    } else {

      leftOffsetRef.current = new Animated.Value(0);

    }

  }, [screenWidth]);



  const determineNewLeftOffset = drawerHeight => {

    return ["top", "left"].includes(placement) ? drawerHeight : -drawerHeight;

  };

  useEffect(() => {

    // open ? openDrawer() : closeDrawer();

    open && setClosebottom(true);

  }, [open]);

  useEffect(() => {

    console.log(closebottom, open);

    if (placement === PopupPlacement.Bottom) {

      open && closebottom && show();

      !open && closebottom && hide();

    } else {

      open && closebottom && openDrawer();

      !open && closebottom && closeDrawer();

    }

  }, [closebottom, open, placement]);



  const show = () => {

    onOpen?.(true);

    Animated.timing(sheetAnimRef, {

      toValue: 0,

      duration: duration,

      useNativeDriver: false

    }).start();

  };



  const hide = () => {

    Animated.timing(sheetAnimRef, {

      toValue: initialDrawerHeight,

      duration: duration,

      useNativeDriver: false

    }).start(() => {

      setClosebottom(false);

    });

  };



  const openDrawer = () => {

    const drawerWidthOrHeight = drawerRef.current;

    const leftOffset = leftOffsetRef.current;

    onOpen?.(true);

    Animated.timing(leftOffset, {

      toValue: determineNewLeftOffset(drawerWidthOrHeight),

      duration: duration,

      useNativeDriver: true

    }).start();

  };



  const closeDrawer = () => {

    const leftOffset = leftOffsetRef.current;

    Animated.timing(leftOffset, {

      toValue: 0,

      duration: duration,

      useNativeDriver: true

    }).start(() => {

      setClosebottom(false);

    });

  };



  const drawerWidth = drawerRef.current;

  const leftOffset = leftOffsetRef.current;



  // const configAggregate = {

  //   bottom: {

  //     width: screenWidth,

  //     positionValue: -(initialDrawerHeight + screenHeight),

  //     animated: { transform: [{ translateY: leftOffset }] }

  //   },

  // };

  const animated = upDownDirection

    ? { transform: [{ translateY: leftOffset }] }

    : { transform: [{ translateX: leftOffset }] };

  return (

    <PopupContext.Provider

      value={{

        open,

        placement,

        emitClose: onClose

      }}

    >

      {placement === PopupPlacement.Bottom ? (

        <Modal visible={closebottom} transparent animationType="none">

          <KeyboardAwareScrollView

            extraHeight={20}

            enableOnAndroid

            enableResetScrollToCoords

            contentContainerStyle={{

              flex: 1,

              alignItems: "center",

              justifyContent: "flex-end"

            }}

          >

            {backdrop}

            <Animated.View

              className={classNames(

                prefixClassname("popup"),

                {

                  [prefixClassname("popup--bottomrounded")]:

                    placement === PopupPlacement.Bottom && rounded

                },

                className

              )}

              style={[

                {

                  width: drawerWithPercentage

                    ? initialDrawerWidth

                    : screenWidth,

                  height: drawerHeightPercentage

                    ? initialDrawerHeight

                    : screenHeight,

                  transform: [{ translateY: sheetAnimRef }]

                },

                { ...style }

              ]}

            >

              {content}

              {close}

            </Animated.View>

          </KeyboardAwareScrollView>

        </Modal>

      ) : (

        <Modal visible={closebottom} transparent animationType="none">

          <KeyboardAwareScrollView

            extraHeight={20}

            enableOnAndroid

            enableResetScrollToCoords

            contentContainerStyle={{

              flex: 1,

              alignItems: "center",

              justifyContent: "center"

            }}

          >

            {backdrop}

            <Animated.View

              className={classNames(

                prefixClassname("popup"),

                {

                  [prefixClassname("popup--toprounded")]:

                    placement === PopupPlacement.Top && rounded,

                  [prefixClassname("popup--leftrounded")]:

                    placement === PopupPlacement.Left && rounded,

                  [prefixClassname("popup--rightrounded")]:

                    placement === PopupPlacement.Right && rounded

                },

                className

              )}

              style={[

                animated,

                placement && {

                  width: drawerWithPercentage

                    ? initialDrawerWidth

                    : screenWidth,

                  height: drawerHeightPercentage

                    ? initialDrawerHeight

                    : screenHeight,

                  [placement]: -drawerWidth

                },

                { ...style }

              ]}

            >

              {close}

              {content}

            </Animated.View>

          </KeyboardAwareScrollView>

        </Modal>

      )}

    </PopupContext.Provider>

  );

};



export default Popup;
```

这是优化有的弹窗，但现在突然出现软键盘往前推太多的情况。

```TypeScript
import {

  useState,

  useEffect,

  useRef,

  Children,

  CSSProperties,

  ReactNode,

  ReactElement,

  isValidElement

} from "react";

import * as _ from "lodash";

import * as React from "react";

import {

  Animated,

  useWindowDimensions,

  Modal

} from "react-native";

import { KeyboardAwareScrollView } from "react-native-keyboard-aware-scroll-view";

import { ITouchEvent } from "@tarojs/components/types/common";

import classNames from "classnames";

import { prefixClassname } from "../../styles";

import PopupContext from "./popup.context";

import PopupBackdrop from "./popup-backdrop";

import PopupClose from "./popup-close";

import { PopupPlacement, PopupPlacementString } from "./popup.shared";

import "./popup.scss";



interface PopupChildren {

  backdrop?: ReactNode;

  close?: ReactNode;

  content: ReactNode[];

}

function findPopupChildren(node?: ReactNode): PopupChildren {

  const children: PopupChildren = {

    backdrop: undefined,

    close: undefined,

    content: []

  };

  Children.forEach(node, (child: ReactNode) => {

    if (isValidElement(child)) {

      const element = child as ReactElement;

      let BackdropStr = PopupBackdrop.toString();

      let PopupCloseStr = PopupClose.toString();

      let elementStr = element.type.toString();

      if (elementStr === BackdropStr) {

        children.backdrop = element;

      } else if (elementStr === PopupCloseStr) {

        children.close = element;

      } else {

        children.content.push(child);

      }

    } else {

      children.content.push(child);

    }

  });



  return children;

}



export interface PopupProps {

  className?: string;

  style?: CSSProperties;

  open: boolean;

  duration?: number;

  rounded?: boolean;

  children?: ReactNode;

  placement?: PopupPlacement | PopupPlacementString;

  onOpen?: (opened: boolean) => void;

  onClose?: (opened: boolean) => void;

  onClosed?: () => void;



  drawerWithPercentage?: number;

  drawerHeightPercentage?: number;

  opacity?: number;

  onClick: (event: ITouchEvent) => void;

  containerHeight?: number;

}

const Popup = (props: PopupProps) => {

  const window = useWindowDimensions();

  const screenHeight = window.height;

  const screenWidth = window.width;

  const {

    className,

    style,

    open,

    placement = "left", //默认为左弹窗

    rounded = false,

    drawerWithPercentage = 100,

    drawerHeightPercentage = 100,

    children,

    onClose,

    onOpen,

    duration = 300

  } = props;



  const { backdrop = <PopupBackdrop />, close, content } = findPopupChildren(

    children

  );

  const initialDrawerWidth = screenWidth * (drawerWithPercentage / 100);

  const initialDrawerHeight = screenHeight * (drawerHeightPercentage / 100);

  // 偏移值

  const leftOffsetRef = useRef(new Animated.Value(0));



  //  背景

  const [upDownDirection] = useState(["top", "bottom"].includes(placement)); //是否为上下方向

  const drawerRef = useRef(

    upDownDirection ? initialDrawerHeight : initialDrawerWidth

  );



  const [closebottom, setClosebottom] = useState(false);

  useEffect(() => {

    drawerRef.current = upDownDirection

      ? initialDrawerHeight

      : initialDrawerWidth;

    if (open) {

      leftOffsetRef.current = new Animated.Value(

        determineNewLeftOffset(drawerRef.current)

      );

    } else {

      leftOffsetRef.current = new Animated.Value(0);

    }

  }, [screenWidth]);



  const determineNewLeftOffset = drawerHeight => {

    return ["top", "left"].includes(placement) ? drawerHeight : -drawerHeight;

  };

  useEffect(() => {

    open && setClosebottom(true);

  }, [open]);

  useEffect(() => {

    open && closebottom && openDrawer();

    !open && closebottom && closeDrawer();

  }, [closebottom, open, placement]);



  const openDrawer = () => {

    const drawerWidthOrHeight = drawerRef.current;

    const leftOffset = leftOffsetRef.current;

    onOpen?.(true);

    Animated.timing(leftOffset, {

      toValue: determineNewLeftOffset(drawerWidthOrHeight),

      duration: duration,

      useNativeDriver: true

    }).start();

  };



  const closeDrawer = () => {

    const leftOffset = leftOffsetRef.current;

    Animated.timing(leftOffset, {

      toValue: 0,

      duration: duration,

      useNativeDriver: true

    }).start(() => {

      setClosebottom(false);

    });

  };



  const drawerWidth = drawerRef.current;

  const leftOffset = leftOffsetRef.current;





  const configAggregate =  {

    positionValue: -drawerWidth,

    animated: upDownDirection? { transform: [{ translateY: leftOffset }] }:{ transform: [{ translateX: leftOffset }] }

  }

  const positionValue = configAggregate?.positionValue ?? 0;

  const animated = configAggregate?.animated ?? {};

  return (

    <PopupContext.Provider

      value={{

        open,

        placement,

        emitClose: onClose

      }}

    >

        <Modal visible={closebottom} transparent animationType="none">

          <KeyboardAwareScrollView

            extraHeight={20}

            enableOnAndroid

            enableResetScrollToCoords

            contentContainerStyle={{

              flex: 1,

              alignItems: "center",

              justifyContent: upDownDirection?"flex-end":"center"

            }}

          >

            {backdrop}

            <Animated.View

              className={classNames(

                prefixClassname("popup"),

                {

                  [prefixClassname("popup--toprounded")]:

                    placement === PopupPlacement.Top && rounded,

                  [prefixClassname("popup--bottomrounded")]:

                    placement === PopupPlacement.Bottom && rounded,

                  [prefixClassname("popup--leftrounded")]:

                    placement === PopupPlacement.Left && rounded,

                  [prefixClassname("popup--rightrounded")]:

                    placement === PopupPlacement.Right && rounded

                },

                className

              )}

              style={[

                animated,

                placement && {

                  width: drawerHeightPercentage?initialDrawerWidth:screenWidth,

                  height: drawerHeightPercentage

                    ? initialDrawerHeight

                    : screenHeight,

                  [placement]: positionValue

                },

                { ...style }

              ]}

            >

              {close}

              {content}

            </Animated.View>

          </KeyboardAwareScrollView>

        </Modal>

    </PopupContext.Provider>

  );

};



export default Popup;
```

踩坑：

一开始KeyboardAwareScrollView标签使用style，结果报错飘红了，翻了翻文档才知道需要使用contentContainerStyle去接收。http://www.sunqizheng.com/blog/659.html



**ActionSheet动作面板**

需支持功能：

1.支持取消

2.中间内容支持禁用，加载

注:结合popup实现

但这里需要考虑使用ios原生面板去实现ActionSheetIOS和安卓面板的展示

当前使用react-native-actionsheet组件库，达到原生端效果兼容，但这个会与非原生端使用不太一样，且样式效果也不相同

有2种设计方法：

第一种是使用modal来完成弹窗效果，另外一种是接入**[react-native-actionsheet](https://github.com/beefe/react-native-actionsheet)****插件**

https://github.com/alessiocancian/react-native-actionsheet

方案一代码：

```TypeScript
import {

    Modal,

    View,

    TouchableOpacity,

    ScrollView,

    StyleSheet,

    Animated,

    Dimensions,

    Text

  } from "react-native";

  

  import { useState, forwardRef, useImperativeHandle } from "react";

  

  const { width, height } = Dimensions.get("window");

  

  const ActionSheetRn = (props: any, ref: any) => {

    const [visible, setVisible] = useState(false);

    const [translateY] = useState(height*0.5);

    const [sheetAnim] = useState(new Animated.Value(translateY));

    useImperativeHandle(ref, () => ({

      show: () => {

        setVisible(true);

        Animated.timing(sheetAnim, {

          toValue: 0,

          duration: 250

        }).start();

      },

      hide: hide

    }));

  

    const hide = () => {

      Animated.timing(sheetAnim, {

        toValue: height - 310,

        duration: 250

      }).start(() => {

        setVisible(false);

      });

    };

  

    const _renderTitle = () => {

      return <Text style={[styles.titleText]}>标题2222222222</Text>;

    };

  

    const _renderContainer = () => {

      return (

        <View>

          <Text>我是内容咯！！！！</Text>

        </View>

      );

    };

  console.log(sheetAnim,'sheetAnims')

    return (

      <Modal

        visible={visible}

        transparent

        animationType="none"

        onRequestClose={hide}

      >

        <View style={styles.wrapper}>

          <TouchableOpacity style={styles.overlay} onPress={hide}>

            <Animated.View

              style={[

                styles.content_container,

                {

                  height: translateY,

                //   transform: [{ translateY: sheetAnim }]

                transform: [{ translateX: sheetAnim }]

                }

              ]}

            >

              {_renderTitle()}

              <ScrollView horizontal showsHorizontalScrollIndicator={false}>

                {_renderContainer()}

              </ScrollView>

            </Animated.View>

          </TouchableOpacity>

        </View>

      </Modal>

    );

  };

  

  const styles = StyleSheet.create({

      wrapper: {

        flex: 1,

        width: width,

        alignItems: "center",

        justifyContent: "flex-end",

        backgroundColor: "rgba(0,0,0,0.5)"

      },

      content_container: {

        width: width,

        backgroundColor: "#fff",

        position: 'relative',

        // position: 'absolute',

        left:0,

        bottom:0

      },

    });

    

  export default forwardRef(ActionSheetRn);
```

方案二：

```TypeScript
import {

  Modal,

  View,

  TouchableOpacity,

  ScrollView,

  StyleSheet,

  Animated,

  Dimensions,

  Text

} from "react-native";

import { ActionSheetCustom as ActionSheet } from "react-native-actionsheet";



import { useState, useRef, forwardRef, useImperativeHandle, useEffect } from "react";



const { width, height } = Dimensions.get("window");

const showCancelBtnArray = ['取消']

const ActionSheetRn = (props: any, ref: any) => {

  const [translateY] = useState(height * 0.5);

  const [sheetAnim] = useState(new Animated.Value(translateY));

  const ActionSheetRnRef = useRef(null);

  const [selectOptions,setSelectOptions]=useState([])//选项

  const [cancelButtonIndex,setCancelButtonIndex]=useState<any>(undefined)//取消关闭弹窗按钮索引

  const [destructiveButtonIndex,setDestructiveButtonIndex]=useState<any>(undefined)//取消关闭弹窗按钮索引



   //默认选择的选项



  useImperativeHandle(ref, () => ({

    show,

    hide

  }));

  const show= () => {

    ActionSheetRnRef.current.show();

  }

  const hide = () => {

    ActionSheetRnRef.current.hide();

  };



  useEffect(()=>{

    props.open && show()

  },[props.open])



  useEffect(()=>{

    if(props.showCancelBtn){

      const containCancelOptions = props.options.concat(showCancelBtnArray)

      setSelectOptions(containCancelOptions)

      setCancelButtonIndex(containCancelOptions.length-1)

      setDestructiveButtonIndex(containCancelOptions.length-1)

    }else{

      setSelectOptions(props.options)

    }

  },[props.options,props.showCancelBtn,props.cancelButtonIndex])





  console.log(cancelButtonIndex,'cancelButtonIndex')

  console.log(selectOptions,'selectOptions')



  return (

    <ActionSheet

      useNativeDriver={false}

      ref={ActionSheetRnRef}

      title={props.title}

      options={selectOptions}

      cancelButtonIndex={cancelButtonIndex}

      destructiveButtonIndex={destructiveButtonIndex}

      onPress={index => {

        /* do something */

        if(index !== cancelButtonIndex){

          props.onSelect(index)

        }else{

          props.onCancel()

        }

        props.onClose()

      }}

      // styles={actionSheetStyle}

    />

  );

};



const actionSheetStyle = StyleSheet.create({

  titleBox: {

    borderTopLeftRadius: 15,

    borderTopRightRadius: 15,

    borderBottomWidth: 1,

    borderBottomColor: "#f7f8fa"

  },

  body: {

    borderTopLeftRadius: 15,

    borderTopRightRadius: 15,

    backgroundColor: "#f7f8fa"

  }

});

export default forwardRef(ActionSheetRn);
```

方案三：接入另一个action-sheet库https://github.com/ammarahm-ed/react-native-actions-sheet

```JavaScript

```

![img](https://xingyun.feishu.cn/space/api/box/stream/download/asynccode/?code=NTMzZTc3ZmZkZmI4MzFjZTdhOGRkM2UyYzZiYjRkOGZfSkxtYmNWNDhZVlBVYlJZdG1xQjU4M3ZnRm55Q0VBN09fVG9rZW46Ym94Y25Eem1ZcGxEUmI4QTMzb3gyWXVFNUhiXzE2NTI2NzE1OTA6MTY1MjY3NTE5MF9WNA)



**dialog(弹出窗)**

弹出方向：所有的组件均从中间弹出

![img](https://xingyun.feishu.cn/space/api/box/stream/download/asynccode/?code=ODQ5NWY1YmYzOWU5NDM1ZDY3YzU0ZmUyYTFiMGYzNGNfdWNWM1RwZloyOFRJT1VUN1EyWXVuVDlVZ2VsY01EYUlfVG9rZW46Ym94Y25BS3pJWDdmNWhPNjBmVkhVa091RkdjXzE2NTI2NzE1OTA6MTY1MjY3NTE5MF9WNA)



 **DropdownMenu下拉菜单**

![img](https://xingyun.feishu.cn/space/api/box/stream/download/asynccode/?code=NDU3MmQ5N2JmMmIxMzVkNjBiZTIyMzNlNjc1YzY2ZTNfUEVMTEE4NktyQnVDZzk2TmdCUTVubWhPN3NkS0tVaVlfVG9rZW46Ym94Y253MFZ5WnVIMkRYcml2TVVHTWJSQXVmXzE2NTI2NzE1OTA6MTY1MjY3NTE5MF9WNA)



**toast轻弹窗**

![img](https://xingyun.feishu.cn/space/api/box/stream/download/asynccode/?code=MjZjMTEwM2Y1YTI3ZTM1MzNmOGRjZDA5NTkzNTQ1NGJfV2Q0VFlud3ZXeXdFNFZHOUNqWGVCYmV4bnY5Zm45T1NfVG9rZW46Ym94Y25veU43RTlOUklmeGtXU2hUUmZBbGxnXzE2NTI2NzE1OTA6MTY1MjY3NTE5MF9WNA)
---
layout: post
title: Objective c动态添加属性
excerpt: "可能是Objective c动态添加属性最简单最好的方式了"
modified: 2013-05-31
categories: articles
tags: [objective c]
comments: true
share: true
author: foxsofter
---

上代码吧

<pre><code>
//
//  NSObject+AssociatedObject.h
//  FXCategories
//
//  Created by fox softer on 15/2/23.
//  Copyright (c) 2015年 foxsofter. All rights reserved.
//

#import <Foundation/Foundation.h>

/**
 *  @author foxsofter, 15-02-23 21:02:44
 *
 *  @brief  添加属性到对象
 */
 @implementation NSObject (AssociatedObject)

 - (id)object:(SEL)key {
   return objc_getAssociatedObject(self, key);
 }

 - (void)setAssignObject:(id)object withKey:(SEL)key {
   objc_setAssociatedObject(self, key, object, OBJC_ASSOCIATION_ASSIGN);
 }

 - (void)setRetainNonatomicObject:(id)object withKey:(SEL)key {
   objc_setAssociatedObject(self, key, object, OBJC_ASSOCIATION_RETAIN_NONATOMIC);
 }

 - (void)setCopyNonatomicObject:(id)object withKey:(SEL)key {
   objc_setAssociatedObject(self, key, object, OBJC_ASSOCIATION_COPY_NONATOMIC);
 }

 - (void)setRetainObject:(id)object withKey:(SEL)key {
   objc_setAssociatedObject(self, key, object, OBJC_ASSOCIATION_COPY_NONATOMIC);
 }

 - (void)setCopyObject:(id)object withKey:(SEL)key {
   objc_setAssociatedObject(self, key, object, OBJC_ASSOCIATION_COPY_NONATOMIC);
 }

 @end
</pre></code>

使用例子

<pre><code>
//
//  UIControl+Block.m
//  WisdomAir
//
//  Created by fox softer on 15/2/23.
//  Copyright (c) 2015年 foxsofter. All rights reserved.
//


@interface UIControl (Block)

- (void)touchDown:(void (^)(void))eventBlock;
- (void)touchDownRepeat:(void (^)(void))eventBlock;
- (void)touchDragInside:(void (^)(void))eventBlock;
- (void)touchDragOutside:(void (^)(void))eventBlock;
- (void)touchDragEnter:(void (^)(void))eventBlock;
- (void)touchDragExit:(void (^)(void))eventBlock;
- (void)touchUpInside:(void (^)(void))eventBlock;
- (void)touchUpOutside:(void (^)(void))eventBlock;
- (void)touchCancel:(void (^)(void))eventBlock;
- (void)valueChanged:(void (^)(void))eventBlock;
- (void)editingDidBegin:(void (^)(void))eventBlock;
- (void)editingChanged:(void (^)(void))eventBlock;
- (void)editingDidEnd:(void (^)(void))eventBlock;
- (void)editingDidEndOnExit:(void (^)(void))eventBlock;

@end

#import "UIControl+Block.h"

// UIControlEventTouchDown           = 1 <<  0,      // on all touch downs
// UIControlEventTouchDownRepeat     = 1 <<  1,      // on multiple touchdowns (tap count > 1)
// UIControlEventTouchDragInside     = 1 <<  2,
// UIControlEventTouchDragOutside    = 1 <<  3,
// UIControlEventTouchDragEnter      = 1 <<  4,
// UIControlEventTouchDragExit       = 1 <<  5,
// UIControlEventTouchUpInside       = 1 <<  6,
// UIControlEventTouchUpOutside      = 1 <<  7,
// UIControlEventTouchCancel         = 1 <<  8,
//
// UIControlEventValueChanged        = 1 << 12,     // sliders, etc.
//
// UIControlEventEditingDidBegin     = 1 << 16,     // UITextField
// UIControlEventEditingChanged      = 1 << 17,
// UIControlEventEditingDidEnd       = 1 << 18,
// UIControlEventEditingDidEndOnExit = 1 << 19,     // 'return key' ending editing
//
// UIControlEventAllTouchEvents      = 0x00000FFF,  // for touch events
// UIControlEventAllEditingEvents    = 0x000F0000,  // for UITextField
// UIControlEventApplicationReserved = 0x0F000000,  // range available for application use
// UIControlEventSystemReserved      = 0xF0000000,  // range reserved for internal framework use
// UIControlEventAllEvents           = 0xFFFFFFFF

#define UICONTROLEVENT(methodName, eventName)                                                      \
  -(void)methodName : (void (^)(void))eventBlock {                                                 \
    [self setCopyNonatomicObject:eventBlock withKey:@selector(methodName:)];                       \
    [self addTarget:self                                                                           \
                  action:@selector(methodName##Action:)                                            \
        forControlEvents:UIControlEvent##eventName];                                               \
  }                                                                                                \
  -(void)methodName##Action : (id)sender {                                                         \
    void (^block)() = [self object:@selector(methodName:)];                                        \
    if (block) {                                                                                   \
      block();                                                                                     \
    }                                                                                              \
  }

@interface UIControl ()

@end

@implementation UIControl (Block)

UICONTROLEVENT(touchDown, TouchDown)
UICONTROLEVENT(touchDownRepeat, TouchDownRepeat)
UICONTROLEVENT(touchDragInside, TouchDragInside)
UICONTROLEVENT(touchDragOutside, TouchDragOutside)
UICONTROLEVENT(touchDragEnter, TouchDragEnter)
UICONTROLEVENT(touchDragExit, TouchDragExit)
UICONTROLEVENT(touchUpInside, TouchUpInside)
UICONTROLEVENT(touchUpOutside, TouchUpOutside)
UICONTROLEVENT(touchCancel, TouchCancel)
UICONTROLEVENT(valueChanged, ValueChanged)
UICONTROLEVENT(editingDidBegin, EditingDidBegin)
UICONTROLEVENT(editingChanged, EditingChanged)
UICONTROLEVENT(editingDidEnd, EditingDidEnd)
UICONTROLEVENT(editingDidEndOnExit, EditingDidEndOnExit)

@end
</pre></code>

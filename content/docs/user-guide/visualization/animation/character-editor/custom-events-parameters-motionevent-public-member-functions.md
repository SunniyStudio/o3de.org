---
description: null
title: MotionEvent 公开成员函数
---

`MotionEvent` 类包括以下公共成员函数。

**主题**
+ [MotionEvent](#char-animation-editor-custom-events-parameters-motionevent)
+ [SetStartTime](#char-animation-editor-custom-events-parameters-setstarttime)
+ [SetEndTime](#char-animation-editor-custom-events-parameters-setendtime)
+ [GetStartTime](#char-animation-editor-custom-events-parameters-getstarttime)
+ [GetEndTime](#char-animation-editor-custom-events-parameters-getendtime)
+ [GetIsTickEvent](#char-animation-editor-custom-events-parameters-getistickevent)
+ [ConvertToTickEvent](#char-animation-editor-custom-events-parameters-converttotickevent)
+ [GetIsSyncEvent](#char-animation-editor-custom-events-parameters-getissyncevent)
+ [SetIsSyncEvent](#char-animation-editor-custom-events-parameters-setissyncevent)
+ [HashForSyncing](#char-animation-editor-custom-events-parameters-hashforsyncing)

## MotionEvent 

您可以使用 `MotionEvent` 函数在特定时间点（tick事件）或指定时间范围（范围事件）内触发事件。要指定事件发出的数据，可以使用指针或数据集。

**语法**

```
MotionEvent (float timeValue, EventDataPtr &&data)
```

创建一个 tick 事件并使用一个数据指针。


****

| 参数 | 说明 |
| --- | --- |
| timeValue | 动作事件发生的时间值（秒）。 |
| data | 事件触发时要输出的值。 |

**语法**

```
MotionEvent (float startTimeValue, float endTimeValue, EventDataPtr &&data)
```

创建一个范围事件并使用一个数据指针。


****

| `参数` | 说明 |
| --- | --- |
| startTimeValue | 动作事件的起始时间（秒）。 |
| endTimeValue | 动作事件结束时的结束时间（秒）。等于起始时间值时，会触发起始事件，但不会发生结束事件。 |
| data | 事件触发时要输出的值。 |

**语法**

```
MotionEvent (float timeValue, EventDataSet &&datas)
```

创建一个 tick 事件并使用一个数据集。


****

| 参数 | 说明 |
| --- | --- |
| timeValue | 动作事件发生的时间值（秒）。 |
| datas | 事件触发时要输出的值。|

**语法**

```
MotionEvent (float startTimeValue, float endTimeValue, EventDataSet &&datas)
```

创建一个范围事件并使用一个数据集。


****

| `参数` | 说明 |
| --- | --- |
| startTimeValue | 动作事件的起始时间（秒）。 |
| endTimeValue | 动作事件结束时的结束时间（秒）。等于起始时间值时，会触发起始事件，但不会发生结束事件。 |
| datas | 事件触发时要输出的值。 |

## SetStartTime 

设置事件的起始时间值，即事件的处理时间。

**语法**

```
void SetStartTime (float timeValue)
```

## SetEndTime 

设置事件的结束时间值，即事件的处理时间。

**语法**

```
void SetEndTime (float timeValue)
```

## GetStartTime 

获取该事件的起始时间值，即事件的执行时间。

**语法**

```
float GetStartTime () const
```

## GetEndTime 

获取此事件的结束时间值，即事件应该停止的时间。

**语法**

```
float GetEndTime () const
```

## GetIsTickEvent 

检查该事件是否为tick事件。

**语法**

```
bool GetIsTickEvent () const
```

## ConvertToTickEvent 

将此事件转换为 tick 事件。

**语法**

```
void ConvertToTickEvent ()
```

## GetIsSyncEvent 

检查该事件是否为同步事件。

**语法**

```
bool GetIsSyncEvent () const
```

## SetIsSyncEvent 

指定此事件是否为同步事件。

**语法**

```
void SetIsSyncEvent (bool newValue)
```

## HashForSyncing 

在动作的同步轨上创建哈希值。

**语法**

```
size_t HashForSyncing (bool isMirror) const
```


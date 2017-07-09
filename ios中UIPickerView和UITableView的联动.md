---
title: ios中UIPickerView和UITableView的联动
date: 2017-05-22 16:03:32
tags: iOS开发
---

### “爱呀”校园项目难点之pickerview与tableview的联动

1. 目标效果

   ![目标效果](https://github.com/JackPanda8/ImageHosting/blob/master/QQ%E5%9B%BE%E7%89%8720170522161901.jpg?raw=true)

   ​

2. 我的思路

   左边使用UIPickerView来实现，右边使用含有7个section的tableview实现。Here Comes 我的心路历程...

   <思路一>

   当用户点击左侧pickerVIew的某一天时，在响应点击的代理方法

   ```
   - (void)pickerView:(UIPickerView *)pickerView didSelectRow:(NSInteger)row inComponent:(NSInteger)component
   {
     [[NSNotificationCenter defaultCenter] postNotificationName:@"ScrollToHeader" object:[NSNumber numberWithLong:row]];
   }
   ```

   ​

   里面，给右侧列表发送通知，右侧列表接收到通知后调用方法：

   ```
   [self.tableView scrollToRowAtIndexPath:[NSIndexPath indexPathForRow:0 inSection:[[noti object] intValue]] atScrollPosition:UITableViewScrollPositionTop animated:YES];
   ```

   将相应的section滚动到top位置。

   右边到左侧的同步则是利用每一个section的header即将display的时候：

   ```
   - (void)tableView:(UITableView *)tableView willDisplayHeaderView:(UIView *)view forSection:(NSInteger)section  {}
   ```

   中给左侧picker发送通知，使其调用

   ```
   [self.dayPicker selectRow:[(NSNumber*)[noti object] intValue] inComponent:0 animated:YES];
   ```

   方法进行同步。

   **该方法存在的问题是：当用户选择左侧的picker时，右边列表滚动过程中会多次调用方法对左侧发送通知，导致其选中的picker会发生偏移,此方法GG**

   <思路二>

   考虑第一种思路下，如果使用当前列表屏幕中的cells的indexpath,找最小的indexpath.section，它就是左侧应当显示的哪一天。所以现在的逻辑就是tableview的属性indexPathsForVisibleRows发生变化的时候给左侧发通知。考虑使用KVO模式，即self注册为自己tableview的观察者，接收到变化时候给左侧发通知。

   **此方法存在的问题是，KVO对数组等集合性质的属性是无效的，因为引用值并不会发生变化，所以此路不通。**

   <思路三>

   左侧的picker调用selectRow:inComponent:animated:方法的情况应当是，当右侧有新的header将要出现(will display)或者有旧的header将要消失(did end display)时，判断当前可见的cells中的indexpath的最小section值。所以最终我的思路是：**picker向table发通知的方式调用table的scrollToRowAtIndexPath:atScrollPosition:animated方法进行滚动，而table向picker则是在WillDisplayHeaderView和didEndDisplayingHeaderView方法中根据tableview的visibleIndexPaths数组寻找当前屏幕中最小的section，如果该最小的section和该TableViewController的属性currentSection不一致，则向picker发送通知，调用其selectRow方法进行滚动。**
# JWComparingTime

* 在tableViewCell.h中:
```objc
//类别模型 (JWText是Model)
@property (nonatomic, strong) JWText *text;
```

* 在tableViewCell.m中:
```objc

-(void)setText:(JWText *)text
{
    _text = text;
    
    //日期格式化类
    NSDateFormatter *fmt = [[NSDateFormatter alloc]init];
    //设置日期格式
    fmt.dateFormat = @"yyyy-MM-dd-HH-mm";
    //传感器创建数据的时间
    NSDate *create = [fmt dateFromString:text.time];
    
    if (create.isThisYear) { //今年
        if (create.isToday) { //今天
            NSDateComponents *cmps = [[NSDate date]deltaFrom:create];
            
            if (cmps.hour >= 1) { //时间差距 >= 1小时
                self.timeLabel.text = [NSString stringWithFormat:@"%zd小时前", cmps.hour];
            }else if (cmps.minute >= 1){ //1小时 > 时间差距 >= 1分钟
                self.timeLabel.text = [NSString stringWithFormat:@"%zd分钟前", cmps.minute];
            }else{ //1分钟 > 时间差距
                self.timeLabel.text = @"刚刚";
            }
        }else if (create.isYesterday){ //昨天
            fmt.dateFormat = @"昨天 HH:mm";
            self.timeLabel.text = [fmt stringFromDate:create];
        }else{ //其他
            fmt.dateFormat = @"MM-dd HH:mm";
            self.timeLabel.text = [fmt stringFromDate:create];
        }
    }else{ //非今年
        //显示正常格式的时间
        fmt.dateFormat = @"yyyy-MM-dd HH:mm";
        self.timeLabel.text = [fmt stringFromDate:create];
    }
}

```

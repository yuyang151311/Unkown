Unkown
## 还没想好


### GCD timer定时器的使用
这里的定时器，是一个每秒在主线程跑的一个方法



	 \_\_block int countSecond = 30; //倒计时
	dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
	dispatch_source_t timer = dispatch_source_create(DISPATCH_SOURCE_TYPE_TIMER, 0, 0,queue);
	dispatch_source_set_timer(timer,dispatch_walltime(NULL, 0),1.0*NSEC_PER_SEC, 0); //每秒执行
	dispatch_source_set_event_handler(timer, ^{
	    if (countSecond==0) { //倒计时完毕
	        //@"倒计时结束，关闭"
	        dispatch_source_cancel(timer);
	        dispatch_async(dispatch_get_main_queue(), ^{
	            //倒计时完毕需要执行的操作
	        });
	    }else{ //倒计时
	        NSLog(@"%@", [NSString stringWithFormat:@"%ld",(long)countSecond]);
	        dispatch_async(dispatch_get_main_queue(), ^{
	            //每秒需要执行的操作
	            //在这里更新UI之类的
	        });
	
	        countSecond--;
	    }
	});
	dispatch_resume(timer);
### 计算文件大小

	(long long)fileSizeAtPath:(NSString \*)path
	{
	NSFileManager *fileManager = [NSFileManager defaultManager];
	
	if ([fileManager fileExistsAtPath:path])
	{
	    long long size = [fileManager attributesOfItemAtPath:path error:nil].fileSize;
	    return size;
	}
	
	return 0;
	}
### 计算文件夹大小

	(long long)folderSizeAtPath:(NSString \*)path
	{
	NSFileManager *fileManager = [NSFileManager defaultManager];
	
	long long folderSize = 0;
	
	if ([fileManager fileExistsAtPath:path])
	{
	    NSArray *childerFiles = [fileManager subpathsAtPath:path];
	    for (NSString *fileName in childerFiles)
	    {
	        NSString *fileAbsolutePath = [path stringByAppendingPathComponent:fileName];
	        if ([fileManager fileExistsAtPath:fileAbsolutePath])
	        {
	            long long size = [fileManager attributesOfItemAtPath:fileAbsolutePath error:nil].fileSize;
	            folderSize += size;
	        }
	    }
	}
	
	return folderSize;
	}
### 向上取整和向下取整

floor(x)函数，是一个向下取整函数，是一个C函数 即是去不大于x的一个最大整数

	floor(3.12) = 3 floor(4.9) = 4

与floor(x)函数对应的是ceil函数
这个即是向上取整了

	ceil(3.9) = 4  ceil(1.2) = 2
	
### 给任何一个view设置一张图片

	UIImage \*image = \[UIImage imageNamed:@"image"];
	self.MYView.layer.contents = (\__bridge id _Nullable)(image.CGImage);
	self.MYView.layer.contentsRect = CGRectMake(0, 0, 0.5, 0.5);

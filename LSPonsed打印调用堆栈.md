打印调用堆栈

{
    super.afterHookedMethod(param);


​    // Hook函数之后执行的代码

​    //函数返回值
​    //XposedBridge.log("afterHookedMethod result:" + param.getResult());

​    // 函数调用完成之后打印堆栈调用的信息
​    // 方法一:
​    Log.i("Dump Stack: ", "---------------start----------------");
​    Throwable ex = new Throwable();
​    StackTraceElement[] stackElements = ex.getStackTrace();
​    if (stackElements != null) {
​        for (int i = 0; i < stackElements.length; i++) {

​            Log.i("Dump Stack" + i + ": ", stackElements[i].getClassName()
                    + "----" + stackElements[i].getFileName()
                                        + "----" + stackElements[i].getLineNumber()
                    
                            + "----" + stackElements[i].getMethodName());
                    }
        ​    }
        ​    Log.i("Dump Stack: ", "---------------over----------------");

​    // 方法二:
​    new Exception().printStackTrace(); // 直接干脆

​    // 方法三:
​    Thread.dumpStack(); // 直接暴力

​    // 方法四:
​    // 打印调用堆栈: http://blog.csdn.net/jk38687587/article/details/51752436
​    RuntimeException e = new RuntimeException("<Start dump Stack !>");
​    e.fillInStackTrace();
​    Log.i("<Dump Stack>:", "++++++++++++", e);

​    // 方法五:
​    // Thread类的getAllStackTraces（）方法获取虚拟机中所有线程的StackTraceElement对象，可以查看堆栈
​    for (Map.Entry<Thread, StackTraceElement[]> stackTrace : Thread.getAllStackTraces().entrySet()) {
​        Thread thread = (Thread) stackTrace.getKey();
​        StackTraceElement[] stack = (StackTraceElement[]) stackTrace.getValue();

​        // 进行过滤
​        if (thread.equals(Thread.currentThread())) {
​            continue;
​        }

​        Log.i("[Dump Stack]", "**********Thread name：" + thread.getName() + "**********");
​        int index = 0;
​        for (StackTraceElement stackTraceElement : stack) {

​            Log.i("[Dump Stack]" + index + ": ", stackTraceElement.getClassName()
                    + "----" + stackTraceElement.getFileName()
                                        + "----" + stackTraceElement.getLineNumber()
                                        + "----" + stackTraceElement.getMethodName());
                        }
                ​        // 增加序列号
                ​        index++;
        ​    }
        ​    Log.i("[Dump Stack]", "********************* over **********************");
}


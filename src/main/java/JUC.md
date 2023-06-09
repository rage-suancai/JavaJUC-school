### 再谈多线程

    JUC相对于Java应用层的学习难度更大 开篇推荐掌握的预备知识: JavaSE多线程部分(必备) 操作系统 JVM(推荐) 计算机组成原理
    掌握预备知识会让你的学习更加轻松 其中 JavaSE多线程部分要求必须掌握 否则无法继续学习本篇🤫 我们不会再去重复教学JavaSE简短的任何知识了

还记得我们在JavaSE中学习的多线程吗? 让我们来回顾一下:

在我们的操作系统之上 可以同时运行很多个进程 并且每个进程之间相互隔离互不干扰 我们的CPU会通过时间片轮转算法
为每一个进程分配时间 并在时间片使用结束后切换下一个进程继续执行 通过这种方式来实现宏观上的多个程序同时运行

由于每个进程都有一个自己的内存空间 进程之间的通信就变得非常麻烦(比如要共享某些数据) 而且执行不同进程会产生上下文切换 非常耗时 那么有没有一种更好地方案呢?

后来 线程横空出世 一个进程可以有多个线程 线程是执行程序中一个单一的顺序控制流程 现在线程才是才行执行流的最小单元
各个线程之间共享程序的内存空间(也就是所在进程的内存空间) 上下文切换速度也高于进程

现在有这样一个问题:

                    public static void main(String[] args) {

                        int[] arr = new int[] {3, 1, 5, 2, 4};
                        // 请将上面的数组按升排输出

                    }

按照正常思维 我们肯定是这样:

                    public static void main(String[] args) {

                        int[] arr = new int[] {3, 1, 5, 2, 4};

                        Arrays.sort(arr); // 直接排序吧
                        for(int i : arr) {
                            System.out.println(i);
                        }

                    }

而我们学习了多线程之后 可以换个思路来实现:

                    public static void main(String[] args) {

                        int[] arr = new int[] {3, 1, 5, 2, 4};

                        for (int i : arr) {
                            new Thread(() -> {
                                try {
                                    Thread.sleep(i * 1000); // 越小的数休眠时间越短 优先被打印
                                    System.out.println(i);
                                } catch (InterruptedException e) {
                                    e.printStackTrace();
                                }
                            })
                        }

                    }

我们接触过的很多框架都在使用多线程 比如Tomcat服务器 所有用户的请求都是通过不同的线程进行处理的
这样我们的网站才可以同时响应多个用户的请求 要是没有多线程 可想而知服务器的处理效率会有多低♿

虽然多线程能够为我们解决很多问题 但是 如何才能正确地使用多线程 如何才能将多线程的资源合理使用 这都是我们需要关心的问题

在Java5的时候 新增了java.util.concurrent(JUC)包 其中包括大量用于多线程编程的工具类 目的是为了了更好的支持高并发任务
让开发者进行多线程编程时减少竞争条件和死锁的问题 通过使用这些工具类 我们的程序更加合理地使用多线程 而我们这一系列视频的主角 正是JUC

但是我们先不着急去看这些内容 第一章 我们先来补点基础知识😯
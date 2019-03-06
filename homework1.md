# 并行程序计算 第一次作业 
## 树形计算伪代码
### 思路
* 最基本的n个核，n个数据，n为2的k次方
   * 第一次以16个核推算  

       *   第一次：0<-1 2<-3 4<-5 ...... 14<-15  
     之后只剩下偶数的核继续操作  
 
       *   第二次：0<-2 4<-6 8<-10 12<-14  
      之后剩下4的倍数
       *   第三次：0<-4 8<-12  
      最后8的倍数
       *   第四次：0<-8
       *   共4次 也就是log2 16
   *  推广到2的k次 n个核  

       *  总共进行k次  

       * 对每i次（0->k-1）    
         step=2的i次方  
       * num[j]+=num[j+step] j from 0 to n-step j+=step in every loop
   * 推广到并行计算  第一次尝试

       ```
       
       int pos = comn_pos //当前核为第几个核 从0开始
       int mySum = input[pos];
       int step = 0;
       int n = comn_size;
       int k = log(2,n);// 2的k次=n
       for(int i=0;i<k;i++)
       {
            step = power(2,i);
          
            if(pos%(2*step) == 0)
            {
                mySum += receiveFrom(pos+step);
            }
            else 
            {
                send(mySum , pos-step);
                break;
            }   
       }

        if(pos = 0)
        {
           print(mySum);
        }
       ```

       上述代码写到一半时就发现似乎存在很大问题，多个核并行计算时有快有慢，receive到的数据很可能是错误的。

    * 经过数分钟的思索，我感觉这样的写法应该是正确的，我疑惑的点应该是在于receive和send 这类进程间通信的函数。 它们应该是保持一定的同步的，只有在收到所需要的数据后，才会继续，不然保持阻塞。  
    估计之后学习的并行计算的库中，会对上述内容进行合理的实现，做出可供我们调用的函数。
    --- 

                                               by 赵奕威 10165101161


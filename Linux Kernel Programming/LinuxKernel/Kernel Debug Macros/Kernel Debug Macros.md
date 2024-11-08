#define FUN_B printk(KERN_INFO" %s_:%s()__%d :  Begins...\n", __FILE__, __func__, __LINE__);  
#define FUN_E printk(KERN_INFO" %s_:%s()__%d :  Ends...\n", __FILE__, __func__, __LINE__);  
#define logs(str) printk(KERN_INFO" %s_:%s()__%d :  %s\n", __FILE__, __func__, __LINE__, str);  

  

  

  

#define FUN_B printf(" %s_:%s()__%d :  Begins...\n", __FILE__, __func__, __LINE__);  
#define FUN_E printf(" %s_:%s()__%d :  Ends...\n", __FILE__, __func__, __LINE__);  
#define logs(str) printf(" %s_:%s()__%d :  %s\n", __FILE__, __func__, __LINE__, str);







![[Pasted image 20241008231733.png]]


![[Pasted image 20241008232458.png]]![[Pasted image 20241011142809.png]]
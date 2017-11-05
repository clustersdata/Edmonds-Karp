# Edmonds-Karp

Edmonds Karp

//求最小割集合的办法:

//设置一个集合A, 最开始A={s},按如下方法不断扩张A:

//1 若存在一条边(u,v), 其流量小于容量，且u属于A,则v加入A

//2 若存在(v,u), 其流量大于0，且u属于A,则v加入A


//A计算完毕，设B=V-A，

//最大流对应的割集E={(u,v) | u∈A,v∈B}

//E为割集，这是一定的


//【最大流】Edmonds Karp算法求最大流,复杂度 O(V E^2)。返回最大流,输入图容量

//矩阵g[M][M],取非零值表示有边，s为源点，t为汇点，f[M][M]返回流量矩阵。


int f[M][M],g[M][M];


int EdmondsKarp(int n,int s,int t){	

	int i,j,k,c,head,tail,flow=0;
  
	int r[M][M];
  
	int prev[M],visit[M],q[M];
  
	for (i=1;i<=n;i++) for (j=1;j<=n;j++) {f[i][j]=0;r[i][j]=g[i][j];} //初始化流量网络和残留网络
  
	while (1) { //在残留网络中找到一条s到t的最短路径
  
		head=tail=0;
    
		memset(visit,0,sizeof(visit));
    
		q[tail++]=s;
    
		prev[s]=-1; visit[s]=1;
    
		while(head!=tail){  //宽度优先搜索从s到t的最短路径
    
			k=q[head++];
      
			for (i=1;i<=n;i++)
      
				if (!visit[i] && r[k][i]>0) {
        
					visit[i]=1;
          
					prev[i]=k;
          
					if (i==t) goto next;
          
					q[tail++]=i;
          
				}
        
		}
    
next:	

		if (!visit[t]) break; //流量已达到最大
    
		c=99999999;
    
		j=t;
    
		while (j!=s) {
    
	i=prev[j];
  
			if (c>r[i][j]) c=r[i][j];
      
			j=i;
      
		}
    
		//下面改进流量
    
		j=t;
    
		while (j!=s) {
    
			i=prev[j];
      
			f[i][j]+=c;
      
			f[j][i]=-f[i][j];
      
			r[i][j]=g[i][j]-f[i][j];
      
			r[j][i]=g[j][i]-f[j][i];
      
			j=i;
      
		}
    
		flow+=c;
    
		//cout << c << endl;
    
	}
  
	return flow;
  
}


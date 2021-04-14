# kruskal-algo
#include<stdio.h>
struct queue{
    int st;
    int nodeno;
    int w;
    struct queue *next;
};
struct queue *front=NULL;
struct queue *rear=NULL;
struct node {
    int vn;
    int wt;
    struct node*next;
};
struct graph {
    int v;
    int e;
    struct node *adj[];
};
void bubsort();
void kruskal(struct graph *);
void create_queue(struct graph* );
struct graph *creategraph();
int main()
{
    int i;
    struct queue*r;
    struct graph*G;
    struct node*t;

    G=creategraph();
    for(i=0; i<G->v; i++)
    {   t=G->adj[i]->next;
        while(t!=G->adj[i])
        {
            printf("%d -- %d=%d\n",G->adj[i]->vn,t->vn,t->wt);

            t=t->next;
        }
    }
    create_queue(G);
    bubsort();
    r=front;
    printf("\n sorted linked list:");
    while(r!=NULL)
    {
        printf("%d %d %d \n",r->st,r->nodeno,r->w);
        r=r->next;
    }
    printf("\n");
    kruskal(G);
    return 0;
}
struct queue *createnode()
{
    struct queue *n;
    n=(struct queue *)malloc(sizeof(struct queue));
    return n;
}
struct graph*creategraph()
{
    int i, x, y,w;
    struct graph *G;
    struct node *temp,*t;
    G=(struct graph*)malloc(sizeof(struct graph));
    printf("enter vertices and edges:");
    scanf("%d %d",&G->v,&G->e);
    for(i=0; i<G->v; i++)
    {
        G->adj[i]=(struct node*)malloc(sizeof(struct node));
        G->adj[i]->vn=i;
        G->adj[i]->wt=-1;
        G->adj[i]->next=G->adj[i];
    }
    
    for(i=0; i<G->e; i++)
    {
        temp=(struct node*)malloc(sizeof(struct node));
        printf("enter source and desti and weight :");
        scanf("%d %d %d",&x,&y,&w);
        temp->vn=y;
        temp->wt=w;
        temp->next=G->adj[x];

        t=G->adj[x];
            while(t->next!=G->adj[x])
            {
                t=t->next;
            }
            t->next=temp;
    }
    return G;
}
void create_queue(struct graph* G)
{
    int i;
    struct node*t;
    struct queue*n;
    for(i=0; i<G->v; i++)
    { 
        t=G->adj[i]->next;
        while(t!=G->adj[i])
        {
            n=createnode();
            n->st=G->adj[i]->vn;
            n->nodeno=t->vn;
            n->w=t->wt;
            n->next=NULL;
            enqueue(n);
            t=t->next;
        }
    }
}
void enqueue(struct queue *n)
{
    if(front==NULL)
    {
        rear=front=n;
    }
    else
    {
        rear->next=n;
        rear=n;
    }
}
struct queue *dequeue()
{
   struct queue *t;
   if(front==rear)
   {
     t=front;
     front=rear=NULL;
   }
   else
    {
     t=front;
     front=front->next;
    }
  return t;
}
void bubsort()
{
   struct queue *i,*j;
   int temp;
   for(i=front;i->next!=NULL;i=i->next)
     {
        for(j=i->next;j!=NULL;j=j->next)
          {
             if(i->w>j->w)
            {
              {
                 temp=i->st;
                 i->st=j->st;
                 j->st=temp;
              }
              {
                 temp=i->nodeno;
                 i->nodeno=j->nodeno;
                 j->nodeno=temp;
              }
              {
                 temp=i->w;
                 i->w=j->w;
                 j->w=temp;
              }
            }
          }
     }
}
void kruskal(struct graph *G)
{
  int a[G->v];
  struct queue *p;
  int i,j,stin,endin;
  for(i=0; i<G->v; i++)
   {
     a[i]=-1;
   }
  while(front!=NULL)
  {
   p=dequeue();
   stin=p->st;
   endin=p->nodeno;
   while(a[stin]!=-1)
    {
      stin=a[stin];          
    }
   while(a[endin]!=-1)  
    {
      endin=a[endin];
    }
    if(stin!=endin)  
     {
      a[endin]=stin;
      printf("%d->%d=%d\n",p->st,p->nodeno,p->w);
     }
   }
}

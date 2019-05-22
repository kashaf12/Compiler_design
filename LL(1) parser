//code@raajtilaksarma
#include<stdio.h>
#include<stdlib.h>
#include<string.h>

int noT,noNT;
char STK[20];
int TOP = -1;
typedef struct rule
{
	char lhs;
	char rhs[10];
	int num;
}rule;
struct rule rules[15];
typedef struct frst
{
	char c;
	char set[30];
	int len;
}frst;
struct frst firsts[10];
typedef struct follow
{
	char c;
	char set[30];
	int len;
	int visited;
}follow;
struct follow follows[10];
int isNonTerminal(char c,int noRules)
{	
	int i=0;
	for(i;i<noRules;i++)
		if(rules[i].lhs==c)
		return 1;
	return 0;
}
void push(char c)
{
	STK[++TOP]=c;
}
void pop()
{
	STK[TOP--]='\0';
}
void printStack()
{
	int i;
	printf("\nTOP=%d\n",TOP);
	for(i=0;i<20;i++)
	{
		if(STK[i]=='\0')
			break;
		printf("%c",STK[i]);
	}	
}
int inarr(char* arr,char c,int noRules)
{
	int i;
	for(i=0;i<noRules;i++)
		if(arr[i]==c)
			return 1;
	return 0;
}
int isInFirSet(char NT, char ch,int noNT)
{
	int i=0;
	int j=0;
	for(i=0;i<noNT;i++)
		if(firsts[i].c==NT)
			break;
	for(j=0;j<=firsts[i].len;j++)		
	if(firsts[i].set[j]==ch)
			return 1;
	
	return 0;
}
int isInFolSet(char NT, char ch,int noNT)
{
	int i=0;
	int j=0;
	for(i=0;i<noNT;i++)
		if(follows[i].c==NT)
			break;
	for(j=0;j<=follows[i].len;j++)		
		if(follows[i].set[j]==ch)
				return 1;
	
	return 0;
}

void copyfirst(char NT1,char NT2,int noNT)
{
	int i,j;
	char ch;
	for(i=0;i<noNT;i++)
		if(firsts[i].c==NT1)
		break;
	for(j=0;j<noNT;j++)
		if(firsts[j].c==NT2)
			break;
	int ind;
	int k;
	for(k=0;k<firsts[j].len;k++)
	{
		ind = firsts[i].len;
		ch = firsts[j].set[k];
		if(!isInFirSet(NT1,ch,noNT)&&ch!='e')
		{
			firsts[i].set[ind]=ch;
			firsts[i].len++;
		}		
	}

}
void copyfollow(char NT1,char NT2,int noNT)
{
	printf("\n");
	int i,j;
	char ch;
	for(i=0;i<noNT;i++)
		if(follows[i].c==NT1)
		break;
	for(j=0;j<noNT;j++)
		if(follows[j].c==NT2)
		break;
	int ind;
	int k;
	for(k=0;k<follows[j].len;k++)
	{
		ind = follows[i].len;
		ch = follows[j].set[k];
		if(!isInFolSet(NT1,ch,noNT)&&ch!='e')
		{
			follows[i].set[ind]=ch;
			follows[i].len++;
		}		
	}

}
void copyfollowfirst(char NT1,char NT2,int noNT)
{
	int i,j;
	char ch;
	for(i=0;i<noNT;i++)
		if(follows[i].c==NT1)
		break;
	for(j=0;j<noNT;j++)
		if(firsts[j].c==NT2)
		break;
	int ind;
	int k;
	for(k=0;k<firsts[j].len;k++)
	{
		ind = follows[i].len;
		ch = firsts[j].set[k];
		if(!isInFolSet(NT1,ch,noNT)&&ch!='e')
		{
			follows[i].set[ind]=ch;
			follows[i].len++;
		}		
	}

}

void copyfollowfollow(char NT1,char NT2,int noNT)
{
	int i,j;
	char ch;
	for(i=0;i<noNT;i++)
		if(follows[i].c==NT1)
		break;
	for(j=0;j<noNT;j++)
		if(follows[j].c==NT2)
		break;
	int ind;
	int k;
	for(k=0;k<follows[j].len;k++)
	{
		ind = follows[i].len;
		ch = follows[j].set[k];
		if(!isInFolSet(NT1,ch,noNT)&&ch!='e')
		{
			follows[i].set[ind]=ch;
			follows[i].len++;
		}		
	}

}

void add2first(char NT1,int noRules,int noNT,int rhslen,int ruleno)
{
	rhslen = rhslen+1;
	//printf("\nNT=%c,ruleno=%d,rhslen=%d\t",NT1,ruleno,rhslen);
	int i=0,k=0;
	int j,index;
	char NT2;
	for(k=0;k<rhslen;)
	{
			char NT2=rules[ruleno].rhs[k];
			if(!isNonTerminal(NT2,noRules))
			{
				//add that terminal to NT1
				for(j=0;j<noNT;j++)
					if(firsts[j].c==NT1)
						break;
				if(!isInFirSet(NT1,NT2,noNT))			
				{
					index = firsts[j].len;
					firsts[j].set[index]=NT2;
					firsts[j].len++;
					return;
				}
				//printf("\n");			
			}
			else if(isNonTerminal(NT2,noRules))
			{
				int temp=0;
				for(j=0;j<noRules;j++)
				{
					if((rules[j].lhs==NT2)&&isNonTerminal(rules[j].rhs[0],noRules))
					{
						temp = 1;
						//printf("\n%c\t%s",rules[j].lhs,rules[j].rhs);
						add2first(NT2,noRules,noNT,rules[j].num,j);	
						copyfirst(NT1,NT2,noRules);	
					}
				}
				if(temp==0)
				{
					copyfirst(NT1,NT2,noRules);
				}
				//printf("\n");	
			}
			if((k==rhslen-1)&&isInFirSet(NT2,'e',noNT))
			{
				//printf("\nk=%d,NT1=%c,NT2=%c\n",k,NT1,NT2);
				int m;
				int index;
				for(m=0;m<noNT;m++)
					if(firsts[m].c==NT1)
						break;
				if(!isInFirSet(NT1,'e',noNT))			
				{
					index = firsts[m].len;
					firsts[m].set[index]='e';
					firsts[m].len++;
				}
				return;
			}
			else if(isInFirSet(NT2,'e',noNT)&&(isNonTerminal(NT2,noRules)))
			{ //printf("JJJJJ\n");
				k++;}
			else {//printf("MMMMM\n");
				return;}
			
	}
	//printf("\n");
	
}
void add2follow(char ch,char NT1,int noNT)
{
	int j,index;
	for(j=0;j<noNT;j++)
		if(follows[j].c==NT1)
			break;
	if(!isInFolSet(NT1,ch,noNT))			
	{
		index = follows[j].len;
		follows[j].set[index]=ch;
		follows[j].len++;
	}
}

int charIndexT(char c,char* term,int noT)
{
	int i;
	if(c=='\0')
		return -1;
	for(i=0;i<noT;i++)
	{
		if(c==term[i])
			return i;
	}
	return -1;
}
int charIndexNT(char c,char* nonTerm,int noNT)
{
	int i;
	if(c=='\0')
		return -1;
	for(i=0;i<noNT;i++)
	{
		if(c==nonTerm[i])
			return i;
	}
}
void printTable(int TABLE[noNT][noT],char* term,char *nonTerm)
{
	printf("\nParsing Table\n\n");
	int i,j;
	for(i=0;i<noT;i++)
		printf("\t%c",term[i]);
	printf("\n");
	for(i=0;i<noNT;i++)
	{
		printf("%c\t",nonTerm[i]);
		for(j=0;j<noT;j++)
			printf("%d\t",TABLE[i][j]);
		printf("\n");
	}
}

void findfollow(int NT1,int noRules,int noNT)
{
	int i,j,k,x,m;
	for(i=0;i<noNT;i++)
		if(follows[i].c==NT1)
		{
			if(follows[i].visited==1)
			{
				return;
			}
			else
			{
				follows[i].visited=1;
				break;
			}
			
		}
	for(j=0;j<noRules;j++)
	{
		char next;
		for(k=0;k<=rules[j].num;k++)
		{
			if(k==rules[j].num)
			{
				if(rules[j].rhs[k]==NT1)
				{
					findfollow(rules[j].lhs,noRules,noNT);
					copyfollowfollow(NT1,rules[j].lhs,noNT);
				}
			}
			else if(rules[j].rhs[k]==NT1)
			{
				x = k;
				next = rules[j].rhs[++x];
				while(x<(rules[j].num+1))
				{
					int temp;
					if(!isNonTerminal(next,noRules))
					{
						add2follow(next,NT1,noNT);
						break;
					}
					else if(isNonTerminal(next,noRules))
					{
						copyfollowfirst(NT1,next,noNT);
						if(isInFirSet(next,'e',noNT))
						{
							if(x==rules[j].num)
							{
								findfollow(rules[j].lhs,noRules,noNT);
								copyfollowfollow(NT1,rules[j].lhs,noNT);	
							}
							else
								next = rules[j].rhs[x+1];
						}
						
					}
					x++;
				}
			
			}
		}
	}	
}
int main()
{
	rules[0].lhs = 'E';
	strncpy(rules[0].rhs,"TD",2);
	rules[0].num = 1;
	rules[1].lhs = 'D';
	strncpy(rules[1].rhs,"+TD",3);
	rules[1].num = 2;
	rules[2].lhs = 'D';
	strncpy(rules[2].rhs,"e",1);
	rules[2].num = 0;
	rules[3].lhs = 'T';
	strncpy(rules[3].rhs,"FU",2);
	rules[3].num = 1;
	rules[4].lhs = 'U';
	strncpy(rules[4].rhs,"*FU",3);
	rules[4].num = 2;
	rules[5].lhs = 'U';
	strncpy(rules[5].rhs,"e",1);
	rules[5].num = 0;
	rules[6].lhs = 'F';
	strncpy(rules[6].rhs,"(E)",3);
	rules[6].num = 2;
	rules[7].lhs = 'F';
	strncpy(rules[7].rhs,"i",1);
	rules[7].num = 0;

	int i,j,k=0;
	int noRules=8;
	char NT,NT1,NT2;
	printf("Rules. . .\n");
	for(i=0;i<noRules;i++)
	{
		printf("%c\t->\t",rules[i].lhs);
		for(j=0;j<(rules[i].num)+1;j++)
			printf("%c",rules[i].rhs[j]);
		printf("\t%d\n",rules[i].num);
	}
	//nonTerminal calc
	char arr[noRules];
	char ch;
	for(i=0;i<noRules;)
	{
		ch = rules[i].lhs;
		if(!inarr(arr,ch,noRules))
			arr[k++]=ch;
		else if(inarr(arr,ch,noRules))
			i++;
	}
	printf("\n\n\n");
	for(i=0;i<k;i++)
	{
		firsts[i].c=arr[i];
		follows[i].c=arr[i];
	}
	int index;
	noNT = k;
	char nonTerm[k];
	for(i=0;i<k;i++)
		nonTerm[i]=arr[i];
	//term calc
	char term[40];
	k=0;
	for(i=0;i<noRules;i++)
	{
		int len;
		len = rules[i].num+1;
		for(j=0;j<=len;j++)
		{
			ch = rules[i].rhs[j];
			if(!isNonTerminal(ch,noRules))
			{
				if((!inarr(term,ch,noRules))&&ch!='\0'&&ch!='e')
				{		
					term[k++]=ch;
				}
			}
		}
	}
	term[k++]='$';
	noT = k;
	//printing non term and term. . .		
	printf("Terminals are. . .\n");
	for(i=0;i<noT;i++)
		printf("%c\t",term[i]);
	printf("\nNon-Terminals are. . .\n");
	for(i=0;i<noNT;i++)
		printf("%c\t",nonTerm[i]);

	//first set calculation
	printf("\n\n");
	for(i=0;i<noRules;i++)
	{
		char first=0;
		char second=0;
		first = rules[i].rhs[0];
		if(rules[i].num!=0)
			second = rules[i].rhs[1];
		if(!isNonTerminal(first,noRules))
		{
			NT = rules[i].lhs;			
			for(j=0;j<noNT;j++)
				if(firsts[j].c==NT)
				break;
			if(!isInFirSet(NT,first,noNT))			
			{
				index = firsts[j].len;
				firsts[j].set[index]=first;
				firsts[j].len++;
			}			
		}

	}
	/*printf("first set. . .\n");
	for(i=0;i<noNT;i++)
	{
		printf("%c\tlen=%d\t%s\n",firsts[i].c,firsts[i].len,firsts[i].set);
	}*/	
	
	for(i=0;i<noRules;i++)
	{
		char first=0;
		char second=0;
		first = rules[i].rhs[0];
		if(rules[i].num!=0)
			second = rules[i].rhs[1];
		if(isNonTerminal(first,noRules))
		{
			NT1 = rules[i].lhs;
			add2first(NT1,noRules,noNT,rules[i].num,i);
			//printf("\n");
			//add2first(NT1,NT2,noRules,noNT);
		}
	}
	/*
	for(i=0;i<noRules;i++)
	{
		char first=0;		
		first = rules[i].rhs[0];
		if(isNonTerminal(first,noRules))
		{
			NT1 = rules[i].lhs;
			NT2 = rules[i].rhs[0];
			printf("\n");
			add2first(NT1,NT2,noRules,noNT);
		}
	}*/
	printf("first set. . .\n");
	for(i=0;i<noNT;i++)
	{
		printf("%c\tlen=%d\t%s\n",firsts[i].c,firsts[i].len,firsts[i].set);
	}	
	//follow set calculation
	add2follow('$',follows[0].c,noNT);
	for(i=0;i<noNT;i++)
	{
		NT1 = follows[i].c;
		findfollow(NT1,noRules,noNT);
		printf("\n");
	}
	printf("follow set. . .\n");
	for(i=0;i<noNT;i++)
	{
		printf("%c\tlen=%d\t%s\n",follows[i].c,follows[i].len,follows[i].set);
	}
	//terminals
	printf("\n\n");
	
	//making table for predictive parsing
	int TABLE[noNT][noT];
	int Tind,NTind;
	for(i=0;i<noNT;i++)
		for(j=0;j<noT;j++)
			TABLE[i][j]=-1;
	for(i=0;i<noRules;i++) 
	{
		NT = rules[i].lhs;
		NT1 = rules[i].rhs[0];
		if(!isNonTerminal(NT1,noRules))
		{
			Tind = charIndexT(NT1,term,noT);
			NTind = charIndexNT(NT,nonTerm,noT);		
			//printf("\n%c(%d),%c(%d),i=%d",NT,NTind,NT1,Tind,i);
			if((Tind!=-1)&&(NTind!=-1))
				TABLE[NTind][Tind] = i;
			if(NT1=='e')
			{
				for(j=0;j<noNT;j++)
					if(follows[j].c==NT)
						break;
				for(k=0;k<follows[j].len;k++)
				{
					ch = follows[j].set[k];
					Tind = charIndexT(ch,term,noT);
					NTind = charIndexNT(NT,nonTerm,noT);		
					//printf("\n%c,%c",NT,ch);
					if((Tind!=-1)&&(NTind!=-1)&&(TABLE[NTind][Tind]==-1))
						TABLE[NTind][Tind] = i;
				}
			}
		}
		else if(isNonTerminal(NT1,noRules))
		{
			for(j=0;j<noNT;j++)
				if(firsts[j].c==NT1)
					break;
			//printf("\n>>%c\n",firsts[j].c);
			for(k=0;k<firsts[j].len;k++)
			{
				ch = firsts[j].set[k];
				Tind = charIndexT(ch,term,noT);
				NTind = charIndexNT(NT,nonTerm,noT);		
				//printf("\n%c,%c",NT,ch);
				if((Tind!=-1)&&(NTind!=-1))
					TABLE[NTind][Tind] = i;
			}
			if(isInFirSet(NT1,'e',noNT))
			{
				for(j=0;j<noNT;j++)
					if(follows[j].c==NT)
						break;
				for(k=0;k<follows[j].len;k++)
				{
					ch = follows[j].set[k];
					Tind = charIndexT(ch,term,noT);
					NTind = charIndexNT(NT,nonTerm,noT);		
					//printf("\n%c,%c",NT,ch);
					if((Tind!=-1)&&(NTind!=-1)&&(TABLE[NTind][Tind]==-1))
						TABLE[NTind][Tind] = i;
				}	
			}
		}
	}
	printTable(TABLE,term,nonTerm);
	printf("\nRule numbers reference. . .\n");
	printf("Rule\tRule no.\n");
	for(i=0;i<noRules;i++)
	{
		printf("%c->",rules[i].lhs);
		for(j=0;j<(rules[i].num)+1;j++)
			printf("%c",rules[i].rhs[j]);
		printf("\t%d\n",i);
	}	
	printf("\n");
	//make parser
	for(i=0;i<20;i++)
	{
		STK[i]='\0';
	}
	//printStack();
	char input[20];
	printf("\nEnter input to check:");
	scanf("%s",input);
	printf("\n");
	k=0;
	char ip,stkTop,temp;
	int rule_no;
	int flag = 0;
	ip = input[k];
	push('E');
	while(1)
	{
		if(TOP==-1&&ip=='$')
		{
			printf("\nSuccessfully parsed!");
			break;
		}
		else if((TOP==-1&&ip!='$'))
		{
			printf("\nInput not successfully parsed!");
			break;
		}		
		stkTop = STK[TOP];
		if(ip=='$')
		{
			for(i=0;i<noRules;i++)
			{
				if(rules[i].lhs==stkTop)
				{
					for(j=0;j<=rules[i].num;j++)
					{
						if(rules[i].rhs[j]=='e')
							flag = 1;
					}	
				}
			}
			if(flag==0)
			{
				printf("\nInput not successfully parsed!");
				break;
			}				
		}
		if(stkTop=='e')
		{
			pop();
		}
		if(isNonTerminal(stkTop,noRules))
		{
			Tind = charIndexT(ip,term,noT);
			NTind = charIndexNT(stkTop,nonTerm,noT);
			if(TABLE[NTind][Tind]!=-1)
			{			
				rule_no = TABLE[NTind][Tind];
				pop();
			}
			else
				break;
			for(i=0;i<noRules;i++)
				if(i==rule_no)
					break;
			for(j=rules[i].num;j>=0;j--)
			{
				temp = rules[i].rhs[j];
				push(temp);
			}	
		}
		else if(!isNonTerminal(stkTop,noRules))
		{
			if(stkTop==ip)
			{
				pop();
				ip = input[++k];
			}
		}
		printf("\nSTACK:%s<-TOP\t\tcurrent-Input-sym:%c",STK,ip);
	}

	printStack();
	printf("\n"); 
	return 0;
}

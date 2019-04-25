#include<stdio.h>
#include<stdlib.h>

struct process{
	int name;
	float bt;
	float at;
	float status;
	float wt;
	float tat;

};

void display(struct process proc[],int num){
	int i;
	printf("The input process details are as shown below\n");
	printf("Process\tBurst-time\tArrival-time\n");
	for(i=0;i<num;i++){
		printf("p%d\t%f\t%f\n",proc[i].name,proc[i].bt,proc[i].at);

	}
	printf("\n");
}

void sort(struct process proc[],int num){
	int i,j;
	struct process temp;
	for(i=0;i<num-1;i++)
    {
        for(j=i+1;j<num;j++)
        {
            if(proc[i].at>proc[j].at){
			
                temp=proc[i];
                proc[i]=proc[j];
                proc[j]=temp;
			}
        }

 	}


}

void schedule(struct process proc[],int num,int sum){
	int i,j;
	float f,avgWaitingTime=0,avgtat=0;
	sort(proc,num);
	printf("\nProcess\tBurst Time\tArrival Time\tWaiting Time\tTurn-Around Time\n");
	for(f=proc[0].at;f<(float)sum;){
		float pr=-9999;
		int nxt;
		float temp;
		for(i=0;i<num;i++){

			if(proc[i].at<=f && proc[i].status!=1){
				temp=(proc[i].bt + (f - proc[i].at)) / proc[i].bt;
				if(pr<temp){

					pr=temp;
					nxt=i;
				}
			}
		}

		f=proc[nxt].bt+f;

		proc[nxt].wt=f-(proc[nxt].at)-(proc[nxt].bt);

		proc[nxt].tat=f-proc[nxt].at;

		avgWaitingTime+=proc[nxt].wt;

		avgtat+=proc[nxt].tat;

		proc[nxt].status=1;

		printf("p%d\t%f\t%f",proc[nxt].name,proc[nxt].bt,proc[nxt].at);
		printf("\t%f\t%f\n",proc[nxt].wt,proc[nxt].tat);

	}
	printf("Average waiting time=%f\n",avgWaitingTime/num);
	printf("Average turn-around time=%f\n",avgtat/num);

}

int main(){
	int i;
	float arrival,burst,sum=0;
	static int num;
	struct process proc[10];

	printf("Enter the number of process:");
	scanf("%d",&num);

	printf("\nNow, enter the data for the %d processes:\n",num);

	
	for(i=0;i<num;i++){

		proc[i].name=i+1;
		printf("Process p%d\n",proc[i].name);

		printf("Burst time:");
		scanf("%f",&burst);
		proc[i].bt=burst;

		printf("Arrival time:");
		scanf("%f",&arrival);
		proc[i].at=arrival;

		proc[i].status=0;
		sum+=proc[i].bt;

		printf("\n");
	}
	
	display(proc,num);

	
	sort(proc,num);

	
	schedule(proc,num,sum);

}

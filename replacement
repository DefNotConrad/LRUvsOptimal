
//  Created by Britain Mackenzie on 11/21/19.
//  Copyright © 2019 Britain Mackenzie. All rights reserved.
//

#include <stdio.h>
#include <stdbool.h>
#include <stdlib.h>
#include <time.h>

//Page table struct, each index in struct array works as page number
struct PAGE{
    int time;
    int frame;
    bool valid;
};

const int numPages = 5;
int ref[20] = {0};
time_t t;

//init char reference array with random
void genString(){
    for(int i=0; i<20; i++){
        ref[i]=((rand() %5)+1);
    }
}

//gets number of frames
int getChoice(int frames){
    bool valid = false;
    
    while(!valid){
        printf("Please enter the number of frames (greater than zero): ");
        scanf("%d", &frames);
        
        valid=true;
        if(frames<0){
            valid=false;
            printf("Invalid number of frames.\n");
        }
    }
    
    valid = false;

    genString();
    return frames;
}


//optimal replacement using foresight
int optimal(struct PAGE pages[], int numFrame){
    int faults = 0;
    int frame[numFrame];
    bool full = false;
    int look,count;
    bool next[numFrame];
    
    printf("OPTIMAL ALGORITHM\nFrames:\n     ");
    for(int i = 0; i<numFrame; i++){
        printf("%d ", (i+1));
        frame[i]=-1;
        next[i]=false;
    }
    printf("\n");
    
    for(int time = 0; time<20; time++){
        //if not end of ref list
        if(ref[time]!=0){
            printf("\nCurrent request: %d\n", ref[time]);
            if(!pages[ref[time]-1].valid){
                faults++;
                if(!full){
                    //find empty spot
                    for(int j =0; j<numFrame; j++){
                        //if empty
                        if(frame[j]==-1 && !pages[(ref[time]-1)].valid){
                            frame[j]=ref[time];
                            pages[(ref[time]-1)].valid = true;
                            pages[(ref[time]-1)].frame = j;
                            if(j==(numFrame-1)){
                                full = true;
                            }
                        }
                    }
                    //find optimal replacement if not in frames
                }else{
                    look = time+1;
                    count=0;
                    while(count<(numFrame-1)){
                        for(int j = 0; j<numFrame; j++){
                            if(frame[j]==ref[look]){
                                //if page hasn't already been counted
                                if(next[j]==false){
                                    next[j]=true;
                                    count++;
                                }
                            }
                        }
                        look++;
                        //if searching over array bounds check
                        if(look==20){
                            count = numFrame;
                        }
                    }
                    count =0;
                    while(next[count]){
                        count++;
                    }
                    
                    pages[frame[count]-1].valid=false;
                    frame[count]=ref[time];
                    pages[ref[time]-1].valid=true;
                    
                    
                    //re-init next flags
                    for(int i =0; i<numFrame;i++){
                        next[i]=false;
                    }
                }
            }
            
            printf("Time %d",time);
            
            printf(" ");
            for(int i = 0; i<numFrame; i++){
                if(i<10){
                    printf("  ");
                }
                printf("%d ", frame[i]);
            }
            printf("\n");
        }
    }
    return faults;
}


/* LRU algorithm determines the least recently used page in a frame to replace when there is a miss. It updates the last time used as a time counter. */
int lru(struct PAGE pages[], int numFrame){
    int faults = 0;
    int frame[numFrame];
    bool full = false;
    int lru = 0;
    int min = 100;
    
    printf("LRU ALGORITHM\nFrames:\n     ");
    for(int i = 0; i<numFrame; i++){
        printf("%d ",i+1);
        frame[i]=-1;
    }
    printf("\n");
    
    //for each number in
    for(int time = 0; time<20; time++){
        //if not end of string
        if(ref[time]!=0){
            printf("\nCurrent request: %d\n", ref[time]);
            //if page is valid in frame is hit
            if(pages[(ref[time]-1)].valid){
                pages[(ref[time]-1)].time = time;
            }else{
                //miss, not in cache
                faults++;
                
                if(!full){
                    //find empty spot
                    for(int j =0; j<numFrame; j++){
                        //if empty
                        if(frame[j]==-1 && !pages[(ref[time]-1)].valid){
                            frame[j]=ref[time];
                            pages[(ref[time]-1)].valid = true;
                            pages[(ref[time]-1)].frame = j;
                            pages[(ref[time]-1)].time = time;
                            if(j==numFrame-1){
                                full = true;
                            }
                        }
                    }
                }else{
                    //find lru
                    for(int j = 0; j<numFrame; j++){
                        if(pages[frame[j]-1].time < min){
                            min = pages[frame[j]-1].time;
                            lru = j;
                        }
                    }
                    min = 100;
                    //replace lru with request, invalidate it, and update new node with frame and time
                    pages[frame[lru]-1].valid = false;
                    frame[lru] = ref[time];
                    pages[ref[time]-1].frame = pages[lru].frame;
                    pages[ref[time]-1].valid = true;
                    pages[ref[time]-1].time = time;
                }
            }
            printf("Time %d|",time);
            printf(" ");
            for(int i = 0; i<numFrame; i++){
                if(i<10){
                    printf("  ");
                }
                printf("%d ",frame[i]);
            }
            printf("\n");
        }
    }
    return faults;
}



int main(void){
    int frames = 0;
    int faults[2];
    srand((unsigned)time(&t));
    struct PAGE pages[numPages] = {0,-1,false};
    
    //get user choice of frame number and ref string
    frames=getChoice(frames);
    
    //display the string provided
    printf("Reference List: ");
    for(int i =0; i<20; i++){
        if(ref[i]!=0){
            printf("%d ",ref[i]);
        }
    }
    printf("\n");
    
    //call LRU and OPTIMAL
    faults[0]=lru(pages,frames);
    
    for(int i= 0; i<numPages; i++){
        pages[i].time=0;
        pages[i].frame=-1;
        pages[i].valid=false;
    }
    
    faults[1]=optimal(pages,frames);
    
    printf("\nPage faults of LRU: %d\nPage faults of Optimal: %d\nDifference: %d\n", faults[0], faults[1], faults[1]-faults[0]);
    
    return 0;
}


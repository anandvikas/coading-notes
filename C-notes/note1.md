# C++ with DSA
### Basic C++ code

    #include<iostream> 
    
    int  main () {
    std::cout<<"Hello world\n";
    return  0;
    }
Preprocessor directive : It is used to include files (example 'iostream' includes the input and output features)

    #include<iostream> 
A 'main' function is mendatory in every c++ program. This function is called at very first by default when the code runs.

"std::" is a keyword which is used to import the standard namespace.

**importing standard namespace globally.**

    #include<iostream> 
    using namespace std; 
### Taking input from user

    #include<iostream>
    using  namespace  std;  
    
    int  main () {
    int  amt1;
    int  amt2;
    cin>>amt1;
    cin>>amt2;
    int  sum = amt1 + amt2;
    cout<<sum;
    return  0;
    }
### Conditionals

    #include<iostream>
    using  namespace  std;  
    
    int  main () {
	    int num1, num2, num3;
	    cin>>num1>>num2>>num3;
	    if(num1>num2){
		    if(num1>num3){
			    cout<<num1;
		    } else {
			    cout<<num3;
		    }
	    } else {
		    if(num2>num3){
			    cout<<num2;
		    } else {
			    cout<<num3;
		    }
	    }
	    return  0;
    }
### Loops
***For loop***

    int  main () {
	    int  inputNum;
	    cin>>inputNum;
	    int  sum = 0;
	    for(int  i = 1 ; i <= inputNum; i++){
		    sum = sum + i;
	    }
	    cout<<sum<<endl;
	    return  0;
    }
***while loop***

    int  main () {
	    int  inputNum;
	    cin>>inputNum;
	    int  sum = 0;
	    int  i = 1;
	    while(i <= inputNum){
		    sum = sum + i;
		    i++;
	    }
	    cout<<sum<<endl;
	    return  0;
    }
***Do-while loop***
This will always run atleast one time irrespective of condition.

    int  main () {
    int  inputNum;
    cin>>inputNum;
    int  sum = 0;
    int  i = 1;
    do{
	    sum = sum + i;
    i++;
    } while (i<=inputNum);
    cout<<sum<<endl;
    return  0;
    }
### Jumps in loops using 'Break' and 'Continue'
These are used to control the flow of loop on meeting some specified condition.
|continue|  break|
|--|--|
| skip to next itration of loop |  is used to terminate the loop|

> to print only even numbers upto 20

    int  main () {
    for(int  i = 1; i<=20; i++){
	    if(i%2!=0){
		    continue;
	    }
	    cout<<i<<endl;
    }
    return  0;
    }

> to print the first prime number between 14-20

    int  main(){
	    for(int  i = 14 ; i <= 20 ; i++){
		    bool  isPrime = true;
		    for(int  deviser = 2 ; deviser<i; deviser++){
			    if(i%deviser==0){
				    isPrime = false;
				    break;
			    }
		    }
		    if(isPrime == true){
			    cout<<i<<endl;
			    break;
		    }
	    }
	    return  0;
    }
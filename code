//University assignment
#include<stdio.h>
#include<stdbool.h>
#include <time.h> 
#define MIN 3990000000U
#define MAX 4010000000U //In ISO C the behaviour of 4294967295 changed between C90 and C99. 
//In C90, on normal systems this is an unsigned value, unsigned int (on 32-bit system) 
//or unsigned long (on 16-bit system). However in C99 this is a signed long long on those systems.


//bibliography:Wikipedia articles for miller-rabin and modular exponentiation,forum Lists Dit UoA

//Deterministic fuction to find primes

bool isPrime_Determ(unsigned int num)
{
 unsigned int  div; //Declaring divisor
 
 int prime=1;// variable initialization
//Prime means flagship for prime numbers
   
                    
    /* if(num==1) prime = 0;  //Conditions in case we start from 1
     if(num==2 || num==3) 
     {
     	return true;
     //	printf("%d \n",num);
	 }*/
    				if ( num % 3 == 0) // Check if number is divided  3.There we uncheck all even numbers and some odds.
      					prime = 0;
     	
		 if(prime==1)
		{// divisor set to 5
		 	for(div = 5; div*div <= num; div=div+6)// If the number is prime then we need to search for divisors only until it's square root.
     		{ 
      				if (num % div == 0 || num % (div + 2) == 0)//Examining frequency of primes numbers I found 
	   				{                                          //example:5,7,11,13,17,19,...
						prime = 0;                          
						break;								//the needed conditions and also the increasing step of the divisor
					}	   
     		}
     	}
    
  		if (prime==1)                 // if number is prime then return true value to increase prime's count
      		return true;
	// else
       return false;		// if number is not prime then return  false value to not increase prime's count
}

//Fuction to find (x^y)%p 
unsigned int  modexp( unsigned long long x,int  y,unsigned int p) 
//Implementing modular exponentiation algorithm
//with right to left binary method and bitwise operators

//With this method we solve the overflow problem when we calculate modulo
{
	unsigned  int result=1;//initialize result
	x = x % p;     //initialize with modulo x
	while(y > 0)  //Examine y's bits until we check all of them(All 0s=0)
	{
		if(y & 1) //Checking the least-significant bit.If the current exponent
				 // is odd we multiply with current x.If it is even we don't.
				 // If y is odd, multiply x with result
		{
			result = (result * x) % p;
		}
		
		// n must be even now
		y = y>>1;  // y = y/2
		x = (x * x) % p; //building exponent avoiding continuous multiplications.
				        //(ex á^5-->á^10 without multiplying 5 times)
						//Shifting the bits of the index-exponent to get the
						//desired modulo of the x^y 

	}
	return result;
}
//Fuction miller-rabin  arguments-> d,current odd number,r
bool milrab(unsigned int  d,unsigned int  n,int  r)
{
	int i,a,j; //Declaring variables 
	unsigned long long  x;
	for(i=0; i<3;i++)
	{
		if(i==0)a=2; // á  can take values 2,7,61 which according to Miller-Rabin are enough 
		if(i==1)a=7; //to determine if the number is prime
		if(i==2)a=61;
		
		x= modexp( a, d, n); //x=a^d(mod n) [1]
		if(x==1||x==n-1)/*Miller-Rabin hypothesis is that for n to be prime either*/
	       continue; //a^d=1(mod n) [ or ] a^((2^r)*d)=-1(mod n)
		
		//if the first equation  holds(1 true) then the or is satisfied and we check another a
		//Note that for an OR to be false we need 2 false values
		//Go to the second part of the or if the first equation doesn't hold(1 false)
		for(j=1;j<r;j++)//j can take values  in range [1,r-1]
		{
			x=modexp(x, 2, n); //x=a^((2^r)*d)(mod n) [2]
			if(x==1)    
			  return false;    //If x never becomes n-1 then n is not prime 
			if(x==n-1)      //Note that if  only for one specific a the equation doesn't hold 
			  break;         //then  a condition returns that the number is composite
		}				//If only for a pair a,j the equation holds then the number is propably prime and either the external loop for a's continues or we get the prime.
		  if(x==n-1)
		  continue;
		  else
		  return false; // None of the steps made x equal n-1.We need to check every possible r for that.
	}
	return true; //If the number pass the miller-rabin set of equations then it's prime.We need to check every possible a for that.
}
//Fuction that prepares values for miller-rabin set of equations
bool isPrime_Propab(unsigned int  n)
{
	if (n<=1||n==4) return false; //Base cases
	if (n<=3) return true;
	
	unsigned int  d ; //Declaring values
	int r=0;
	
	if(n & 1)  // Checking number if only it is odd.Even numbers can't be primes
	{
		d=n-1;  //Making the [odd] number [even]
		while(!(d&1))//Dividing d with 2 until it becomes odd(d % 2 == 0)
	    {
			d = d >> 1;      //We know every even number can be written in the form
	 		r+= 1;        // (2^r)*d  
	    }
         if (!milrab(d, n, r)) //Calling miller-rabin fuction
            return false;   //If it returns false,then the number is not prime.
		else//So Propabilistic fuctions exit with false.
			return true;
			//If it returns true then it's prime
	}
	else
	return false;//If number is even for sure its not prime
}

int main()
{
clock_t start_t, end_t, total_t;   //Preparing clock fuction and implementing it
start_t = clock();				  // to find  the running time of the program
printf("Checking range [3990000000,4010000000] for primes \n");
unsigned int  num;
int primes=0;
	for (num=MIN|1; num<=MAX; num+=2)//Check only odd numbers with step 2
	{ 
		if(isPrime_Determ(num)) //Searching for primes in the given range of numbers
		{						//using deterministic fuction
			primes++;
		}
	}
end_t = clock();
float time = (float) (end_t-start_t) / CLOCKS_PER_SEC; //Finding total running time
printf("Deterministic algorithm: Found %d primes in %3.2f secs \n",primes,time); 


start_t = clock();
primes=0;
	for(num=MIN|1;num<=MAX;num++) //Searching for primes in the given range of numbers
	{							//using miller-rabin propabilistic fuction
		if(isPrime_Propab(num))
		{
			primes++;
		}
	}

end_t = clock();
time = (float) (end_t-start_t) / CLOCKS_PER_SEC;
printf("Miller-Rabin algorithm  : Found %d primes in %3.2f secs \n", primes,time);
	return 0;
}

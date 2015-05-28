//8.18
zain ali asim
#include <iostream>
#include <iomanip>
#include <stdlib.h>
using namespace std;
const int SIZE = 100, MAX_WORD = 9999, MIN_WORD = -9999;
const long SENTINEL = -99999;
enum Commands {
	READ = 10, WRITE, LOAD = 20, STORE, ADD = 30, SUBTRACT,
	DIVIDE, MULTIPLY, BRANCH = 40, BRANCHNEG, BRANCHZERO, HALT
};

void load(int * const);
void execute(int * const, int * const, int * const, int * const,

	int * const, int * const);
void dump(const int * const, int, int, int, int, int);
bool validWord(int);
void sum(int a);
void average(int arr[7]);
void series(int arr1,int n);

int main()
{
	int memory[SIZE] = { 0 }, accumulator = 0, instructionCounter = 0,
		opCode = 0, operand = 0, instructionRegister = 0, a = 0;
	int arr[7] = { 24, 5, 7, -9, 8, -10, -4 };

	load(memory);
	execute(memory, &accumulator, &instructionCounter, &instructionRegister,
		&opCode, &operand);

	dump(memory, accumulator, instructionCounter, instructionRegister,
		opCode, operand);
	sum(a);
	average(arr);
	int n = 0;
	cout << "here am using an arry(uswer defined size as series of numbers)" << endl;
	cout << "enter the size of the series" << endl;
	cin >> n;
	int *arr2 = new int[n];
	series(*arr2,n);
	return 0;
}

void load(int * const loadMemory)
{
	long instruction;
	int i = 0;


	cout << "your program. \n" << "00 ? ";
	cin >> instruction;


	while (instruction != SENTINEL)
	{

		if (!validWord(instruction))
			cout << "Number out of range. Please enter again.\n";
		else
			loadMemory[i++] = instruction;
		// function setfill sets the padding character for unused
		// field widths.
		cout << setw(2) << setfill('0') << i << " ? ";
		cin >> instruction;

	}
}

void execute(int * const memory, int * const acPtr, int * const icPtr,
	int * const irPtr, int * const opCodePtr, int * const opPtr)
{
	bool fatal = false;
	int integer;
	const char *messages[] = { "Accumulator overflow ***",
		"Attempt to divide by zero ",
		"Invalid opcode detected " },
		*termString = "\n Simpletron execution abnormally terminated ",
		*fatalString = " FATAL ERROR: ";

	cout << "\nSTART SIMPLETRON EXECUTION|n\n";

	do {
		*irPtr = memory[*icPtr];
		*opCodePtr = *irPtr / 100;



		*opPtr = *irPtr % 100;

		switch (*opCodePtr) {
		case READ:
			cout << "Enter an integer: ";
			cin >> integer;

			while (!validWord(integer))
			{
				cout << "Number out of range. Please enter again: ";
				cin >> integer;

			}

			memory[*opPtr] = integer;
			++(*icPtr);

			break;
		case WRITE:
			cout << "Contents of " << setw(2) << setfill('0') << *opPtr
				<< ": " << memory[*opPtr] << '\n';
			++(*icPtr);
			break;
		case LOAD:
			*acPtr = memory[*opPtr];
			++(*icPtr);
			break;
		case STORE:
			memory[*opPtr] = *acPtr;
			++(*icPtr);
			break;
		case ADD:
			integer = *acPtr + memory[*opPtr];
			cout << "sum is" << integer << endl;

			if (!validWord(integer)) {
				cout << fatalString << messages[0] << termString << '\n';
				fatal = true;
			}
			else {
				*acPtr = integer;
				++(*icPtr);
			}

			break;
		case SUBTRACT:
			integer = *acPtr - memory[*opPtr];
			cout << "on subtracting" << integer << endl;

			if (!validWord(integer)) {
				cout << fatalString << messages[0] << termString << '\n';
				fatal = true;
			}
			else {
				*acPtr = integer;
				++(*icPtr);
			}

			break;
		case DIVIDE:
			if (memory[*opPtr] == 0) {
				cout << fatalString << messages[1] << termString << '\n';
				fatal = true;
			}
			else {
				*acPtr /= memory[*opPtr];
				++(*icPtr);
			}


			break;
		case MULTIPLY:
			integer = *acPtr * memory[*opPtr];
			cout << "product is" << integer << endl;

			if (!validWord(integer)) {
				cout << fatalString << messages[0] << termString << '\n';
				fatal = true;
			}
			else {
				*acPtr = integer;
				++(*icPtr);
			}
			break;
		case BRANCH:
			*icPtr = *opPtr;
			break;
		case BRANCHNEG:
			if (*acPtr < 0)
				*icPtr = *opPtr;
			++(*icPtr);
			break;
		case BRANCHZERO:
			*acPtr == 0 ? *icPtr = *opPtr : ++(*icPtr);
			break;
		case HALT:
			cout << "*** Simpletron execution terminated ***\n";
			break;
		default:
			cout << fatalString << messages[2] << termString << '\n';
			fatal = true;
			break;
		}
	} while (*opCodePtr != HALT && !fatal);

	cout << "\n*************END SIMPLETRON EXECUTION*************\n";
}

void dump(const int * const memory, int accumulator, int instructionCounter,
	int instructionRegister, int operationCode, int operand)
{
	void output(const char * const, int, int, bool); // prototype

	cout << "\nREGISTERS:\n";
	output("accumulator", 5, accumulator, true);
	output("instructionCounter", 2, instructionCounter, false);
	output("instructionRegister", 5, instructionRegister, true);
	output("operationCode", 2, operationCode, false);
	output("operand", 2, operand, false);


}
bool validWord(int word)
{
	return word >= MIN_WORD && word <= MAX_WORD;
}

void output(const char * const sPtr, int width, int value, bool sign)
{
	// format of "accumulator", etc.
	cout << setfill(' ') << setiosflags(ios::left) << setw(20)
		<< sPtr << ' ';

	// is a +/- sign needed?
	if (sign)
		cout << setiosflags(ios::showpos | ios::internal);

	// setup for displaying accumulator value, etc.
	cout << resetiosflags(ios::left) << setfill('0');

	// determine the field widths and display value
	if (width == 5)
		cout << setw(width) << value << '\n';
	else // width is 2
		cout << setfill(' ') << setw(3) << ' ' << setw(width)
		<< setfill('0') << value << '\n';

	// disable sign if it was set
	if (sign)
		cout << resetiosflags(ios::showpos | ios::internal);
}

void sum(int a)
{
	int b;
	cout << "Enter the positive integers to find sum(two integers)" << endl;
	cin >> a;
	cin >> b;
	while (a > 0 && b > 0)
	{

		cout << "sum is\t" << (a + b) << endl;

		break;

	}
	if (a < 0 || b < 0)
	{


		cout << "your input integer was negative" << endl;
	}
}
void average(int arr[7])
{
	cout << "to find the average of seven numbers" << endl;
	int sum = 0;
	int i = 0;
	int avg = 0;

	while (i < 7)
	{

		sum += arr[i];
		avg = (sum / 7);
		

		i += 1;
		

	}
	cout <<"AVERAGE IS\t"<< avg << endl;

}



void series(int arr1, int n)
{

	int *arr2 = new int[n];
	int i = 0;

	while (i < n)
	{
		arr2[i] = rand() % 100;

		cout << *(arr2 + i) << endl;
		i += 1;
	}

	int c = 0;
	for (int i = 0; i<n; i++)
	{
		if (*(arr2 + c) > *(arr2 + i))
		{
			int *p = arr2;
			cout << "the largest one is" << *p << endl;


			if (*(arr2 + c) < *(arr2 + i))
			{
			int *p =arr2 +i;
				cout << "largest is" << *p << endl;
				c += 1;
			}

		}

	
	}
	
	



	delete arr2;

}


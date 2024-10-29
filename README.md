# Assignment-3
#include <iostream>

using namespace std;

const int SIZE = 1000;

struct OperationSteps {
    long steps = 0; // Counts key operations like comparisons and assignments
};

// Bubble Sort
void bubbleSort(int arr[], int n, OperationSteps& ops) {
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            ops.steps++; // Comparison
            if (arr[j] > arr[j + 1]) {
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
                ops.steps += 3; // Assignment
            }
        }
    }
}

// Selection Sort
void selectionSort(int arr[], int n, OperationSteps& ops) {
    for (int i = 0; i < n - 1; i++) {
        int minIdx = i;
        for (int j = i + 1; j < n; j++) {
            ops.steps++; // Comparison
            if (arr[j] < arr[minIdx]) {
                minIdx = j;
            }
        }
        if (minIdx != i) {
            int temp = arr[minIdx];
            arr[minIdx] = arr[i];
            arr[i] = temp;
            ops.steps += 3; // Assignment
        }
    }
}

// Merge Sort helper functions
void merge(int arr[], int left, int mid, int right, OperationSteps& ops) {
    int n1 = mid - left + 1;
    int n2 = right - mid;
    int L[n1], R[n2];

    for (int i = 0; i < n1; i++) { L[i] = arr[left + i]; ops.steps++; }
    for (int i = 0; i < n2; i++) { R[i] = arr[mid + 1 + i]; ops.steps++; }

    int i = 0, j = 0, k = left;
    while (i < n1 && j < n2) {
        ops.steps++; // Comparison
        if (L[i] <= R[j]) arr[k++] = L[i++];
        else arr[k++] = R[j++];
        ops.steps++; // Assignment
    }
    while (i < n1) { arr[k++] = L[i++]; ops.steps++; }
    while (j < n2) { arr[k++] = R[j++]; ops.steps++; }
}

void mergeSort(int arr[], int left, int right, OperationSteps& ops) {
    if (left >= right) return;
    int mid = left + (right - left) / 2;
    mergeSort(arr, left, mid, ops);
    mergeSort(arr, mid + 1, right, ops);
    merge(arr, left, mid, right, ops);
}

// Quick Sort helper function
int partition(int arr[], int low, int high, OperationSteps& ops) {
    int pivot = arr[high];
    int i = low - 1;
    for (int j = low; j <= high - 1; j++) {
        ops.steps++; // Comparison
        if (arr[j] < pivot) {
            i++;
            int temp = arr[i];
            arr[i] = arr[j];
            arr[j] = temp;
            ops.steps += 3; // Assignment
        }
    }
    int temp = arr[i + 1];
    arr[i + 1] = arr[high];
    arr[high] = temp;
    ops.steps += 3; // Assignment
    return i + 1;
}

void quickSort(int arr[], int low, int high, OperationSteps& ops) {
    if (low < high) {
        int pi = partition(arr, low, high, ops);
        quickSort(arr, low, pi - 1, ops);
        quickSort(arr, pi + 1, high, ops);
    }
}

// Helper function to display results
void displayResult(const string& sortName, const string& caseType, OperationSteps& ops) {
    cout << sortName << " on " << caseType << " array - Steps: " << ops.steps << endl;
}

// Helper function to run each sorting algorithm
void runSortingTest(void (*sortFunction)(int[], int, OperationSteps&), int arr[], int n, const string& sortName, const string& caseType) {
    int tempArr[SIZE];
    for (int i = 0; i < n; i++) tempArr[i] = arr[i];
    
    OperationSteps ops;
    sortFunction(tempArr, n, ops);
    displayResult(sortName, caseType, ops);
}

void runSortingTest(void (*sortFunction)(int[], int, int, OperationSteps&), int arr[], int n, const string& sortName, const string& caseType) {
    int tempArr[SIZE];
    for (int i = 0; i < n; i++) tempArr[i] = arr[i];
    
    OperationSteps ops;
    sortFunction(tempArr, 0, n - 1, ops);
    displayResult(sortName, caseType, ops);
}

int main() {
    int bestCase[SIZE], averageCase[SIZE], worstCase[SIZE];

    // Initialize arrays for best, average, and worst cases
    for (int i = 0; i < SIZE; i++) {
        bestCase[i] = i;                   // Sorted array (Best Case)
        averageCase[i] = rand() % SIZE;    // Random array (Average Case)
        worstCase[i] = SIZE - i;           // Reverse sorted array (Worst Case)
    }

    // Measure and display steps for each sorting algorithm on each case
    cout << "Bubble Sort Results:\n";
    runSortingTest(bubbleSort, bestCase, SIZE, "Bubble Sort", "Best Case");
    runSortingTest(bubbleSort, averageCase, SIZE, "Bubble Sort", "Average Case");
    runSortingTest(bubbleSort, worstCase, SIZE, "Bubble Sort", "Worst Case");

    cout << "\nSelection Sort Results:\n";
    runSortingTest(selectionSort, bestCase, SIZE, "Selection Sort", "Best Case");
    runSortingTest(selectionSort, averageCase, SIZE, "Selection Sort", "Average Case");
    runSortingTest(selectionSort, worstCase, SIZE, "Selection Sort", "Worst Case");

    cout << "\nMerge Sort Results:\n";
    runSortingTest(mergeSort, bestCase, SIZE, "Merge Sort", "Best Case");
    runSortingTest(mergeSort, averageCase, SIZE, "Merge Sort", "Average Case");
    runSortingTest(mergeSort, worstCase, SIZE, "Merge Sort", "Worst Case");

    cout << "\nQuick Sort Results:\n";
    runSortingTest(quickSort, bestCase, SIZE, "Quick Sort", "Best Case");
    runSortingTest(quickSort, averageCase, SIZE, "Quick Sort", "Average Case");
    runSortingTest(quickSort, worstCase, SIZE, "Quick Sort", "Worst Case");

    return 0;
}

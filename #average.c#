/* File: average.c
 * ---------------
 * A test program to experiment with different ways of calculating the average
 * of a sequence of floating point numbers. You are free to make changes to
 * this program for your own experiments. Try constructing sequences of your 
 * own or try to make your own variant of average to further explore floats.
 */
#include <errno.h>
#include <error.h>
#include <float.h>
#include <math.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>


/* Comparator to order floats in increasing order.
 * Same as the compare_doubles GNU function listed in lab4,
 * except for floats.  How/why does this work?
 */
int cmp_flt(const void *p1, const void *p2) {
    float p1_float = *(float *)p1;
    float p2_float = *(float *)p2;
    return (p1_float > p2_float) - (p1_float < p2_float);
}

// Comparator to order floats in decreasing absolute value order
int rev_abs_cmp_flt(const void *p1, const void *p2) {
    float p1_float_abs = fabsf(*(float *)p1);
    float p2_float_abs = fabsf(*(float *)p2);
    return ((p2_float_abs > p1_float_abs) - (p2_float_abs < p1_float_abs));
}

/* Functions: average, averageA, averageB, averageC
 * -----------------------------------------------
 * Below are four different versions of average, one naive and
 * 3 "improved" versions.
 */

float average(const float arr[], int n) {
    float sum = 0;
    for (int i = 0; i < n; i++) {
        sum += arr[i];
    }
    return sum / n;
}

float averageA(const float arr[], int n) {
    float sum = 0;
    for (int i = 0; i < n; i++) {
        sum += arr[i] / n;
    }
    return sum;
}

float averageB(const float arr[], int n) {
    // don't re-arrange caller's version of array, make copy
    float copy[n];
    memcpy(copy, arr, sizeof(copy));
    qsort(copy, n, sizeof(copy[0]), cmp_flt);
    
    float sum = 0;
    for (int i = 0; i < n; i++) {
        sum += copy[i];
    }
    return sum / n;
}

float averageC(const float arr[], int n) {
    // don't re-arrange caller's version of array, make copy
    float copy[n];
    memcpy(copy, arr, sizeof(copy));
    qsort(copy, n, sizeof(copy[0]), rev_abs_cmp_flt);
    
    float sum = 0;
    for (int i = 0; i < n; i++) {
        sum += copy[i];
    }
    return sum / n;
}

/* This function tests/outputs results from the different average functions
 * on the provided array of floats with the given size.  It outputs
 * the specified test ID number, the contents of the array, and the
 * results of each average calculation.
 */
void test_sequence(int id, float arr[], int count) {
    float naive = average(arr, count);
    float a = averageA(arr, count);
    float b = averageB(arr, count);
    float c = averageC(arr, count);
    printf("\nSequence #%d [", id);
    for (int i = 0; i < count; i++) {
        printf("%g", arr[i]);
        if (i < count - 1) {
            printf(", ");
        }
    }
    printf("] average\n  naive= %-14.9g  a= %-14.9g  b= %-14.9g  c= %-14.9g\n",
           naive, a, b, c);
}


// This function runs average tests on several hardcoded float arrays.
void fixed_sequences() {
    float seq1[] = {4, 4.5, 5, 0};
    float seq2[] = {FLT_MAX, FLT_MAX};
    float seq3[] = {33333330, 66666660, 9};
    float seq4[] = {1, 1, 1, 1, 1, 1, 1, 1, 1, 1};
    float seq5[] = {66666666, 3, -66666666};
    float seq6[] = {1 << 24, 1, 0, -1};
    float seq7[] = {1.5e8, 100.5, -1.5e8, 99.5};
    float seq8[] = {1e-45, 2e-45};
    // add your own sequences here!

    test_sequence(1, seq1, sizeof(seq1) / sizeof(seq1[0]));
    test_sequence(2, seq2, sizeof(seq2) / sizeof(seq2[0]));
    test_sequence(3, seq3, sizeof(seq3) / sizeof(seq3[0]));
    test_sequence(4, seq4, sizeof(seq4) / sizeof(seq4[0]));
    test_sequence(5, seq5, sizeof(seq5) / sizeof(seq5[0]));
    test_sequence(6, seq6, sizeof(seq6) / sizeof(seq6[0]));
    test_sequence(7, seq7, sizeof(seq7) / sizeof(seq7[0]));
    test_sequence(8, seq8, sizeof(seq8) / sizeof(seq8[0]));
}

/* If no additional arguments are specified, this program will run
 * hardcoded tests in the fixed_sequences() function.  Otherwise,
 * it will read in the specified float command-line arguments and
 * use these numbers as a test array for the average functions.
 */
int main(int argc, char *argv[]) {
    if (argc <= 1) {
        fixed_sequences();
    } else {
        float arr[argc - 1];
        
        // convert string arguments to float values
        for (int i = 1; i < argc; i++) {
            char *end;
            arr[i - 1] = strtof(argv[i], &end);
            if (*end != '\0' || errno) {
                error(1, errno, "invalid number '%s'", argv[i]);
            }
        }
        test_sequence(0, arr, argc - 1);
    }
    return 0;
}

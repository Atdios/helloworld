
#include <iostream>
using namespace std;

// 插入排序  
void insertSort(int R[], int low, int high) {
	int i, j, tmp;
	for (i = low + 1; i <= high; ++i) {
		tmp = R[i];
		j = i - 1;
		while (j >= low && R[j] > tmp) {
			R[j + 1] = R[j];
			--j;
		}
		R[j + 1] = tmp;
	}
}

// 递归寻找中位数的中位数  
int FindMid(int R[], int low, int high) {
	// 只有一个元素
	if (low == high) {
		return R[low];
	}
	int i, k;
	// 将序列划分为5个元素一组，分别求取中位数
	for (i = low; i + 4 <= high; i += 5) {
		insertSort(R, i, i + 4);
		k = i - low;
		// 将中位数交换到前面
		swap(R[low + k / 5], R[i + 2]);
	}
	int n = high - i + 1;
	// 最后一组不足5个元素
	if (n > 0) {
		insertSort(R, i, high);
		k = i - low;
		swap(R[low + k / 5], R[i + n / 2]);
	}
	k = k / 5;
	// 只有一个中位数
	if (k == 0) {
		return R[low];
	}
	return FindMid(R, low, low + k);
}

// 寻找中位数的所在位置  
int FindId(int R[], int low, int high, int median) {
	for (int i = low; i <= high; ++i) {
		if (median == R[i]) {
			return i;
		}
	}
	return -1;
}

//进行划分过程  
int Partion(int R[], int low, int high, int index) {
	if (low <= high) {
		// 将中位数与第1个元素交换
		swap(R[index], R[low]);
		int tmp = R[low];
		int i = low, j = high;
		// 将小于中位数的元素交换到中位数的左边，大于中位数的元素交换到中位数的右边
		while (i != j) {
			while (j > i&&R[j] >= tmp) {
				--j;
			}
			R[i] = R[j];
			while (i < j&&R[i] <= tmp) {
				++i;
			}
			R[j] = R[i];
		}
		R[i] = tmp;
		return i;
	}
	return -1;
}

int BFPTR(int R[], int low, int high, int K){
	// 中位数
	int median = FindMid(R, low, high);
	// 中位数下标
	int index = FindId(R, low, high, median);
	// 划分，得到中位数新的下标
	int newIndex = Partion(R, low, high, index);
	// 中位数在当前序列[low..high]中的位置
	int rank = newIndex - low + 1;
	if (rank == K) {
		// 中位数是第K小元素
		// 左边的K个元素（包括中位数）为Top-K
		// 返回中位数下标
		return newIndex;
	}
	else if (rank > K) {
		return BFPTR(R, low, newIndex - 1, K);
	}
	return BFPTR(R, newIndex + 1, high, K - rank);
}

int main(int argc, char** argv)
{
    const int N = 12;
    int i;
    int R[] = { 12, 1, 8, 10, 6, 2, 5, 9, 11, 3, 4, 7 };
    for (i = 0; i < N; ++i)
    {
        cout << R[i] << " ";
    }
    cout << endl << endl;
    int K, index;
    int R1[N];
    for (int t = 1; t <= 12; ++t)
    {
        K = t;
        for (i = 0; i < N; ++i)
        {
            R1[i] = R[i];
        }
        index = BFPTR(R1, 0, N - 1, K);
        for (i = 0; i < N; ++i)
        {
            cout << R1[i] << " ";
        }
        cout << endl;
        cout <<  K << R1[index] << endl;
        cout <<  K;
        // 对Top-K元素进行排序，方便查看
        insertSort(R1, index - K + 1, index);
        for (i = index - K + 1; i <= index; ++i)
        {
            cout << R1[i] << " ";
        }
        cout << endl << endl;
    }
    return 0;
}

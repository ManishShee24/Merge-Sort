# Merge-Sort
Merge Sort: Counting Inversions
#include <bits/stdc++.h>

using namespace std;

vector<string> split_string(string);
  
long merge(vector<int> &v, int l, int m, int r)
{
    int n1 = m-l+1;
    int n2 = r-m;
    long inv = 0;
    vector<int> L(n1);
    vector<int> R(n2);
    for(int i = 0,j = l; i<n1; i++,j++)
        L[i] = v[j];
    for(int i = 0,j=m+1; i<n2 ; i++,j++)
        R[i] = v[j];
    int i = 0, j = 0,k = l;
    while(i != n1 && j!= n2)
    {
        if(L[i] <= R[j])
            v[k] = L[i++];
        else
        {
            v[k] = R[j++];
            inv += n1-i;
        }
        k++;
    }
    while(i<n1)
        v[k++] = L[i++];

    while(j<n2)
        v[k++] = R[j++];
    return inv;
}

long mergeSort(vector<int> &v, int l, int r)
{
    long inv = 0;
    if(l < r)
    {
        int  m = (l+r)/2;
        long inv1 = mergeSort(v, l, m);
        long inv2 = mergeSort(v, m+1, r);
        inv = inv1+inv2+merge(v, l, m ,r);
    }
    return inv;
}

long countInversions(vector<int> v)
{
    int l = 0, r=v.size()-1;
    return mergeSort(v,l,r);
}

int main()
{
    ofstream fout(getenv("OUTPUT_PATH"));

    int t;
    cin >> t;
    cin.ignore(numeric_limits<streamsize>::max(), '\n');

    for (int t_itr = 0; t_itr < t; t_itr++) {
        int n;
        cin >> n;
        cin.ignore(numeric_limits<streamsize>::max(), '\n');

        string arr_temp_temp;
        getline(cin, arr_temp_temp);

        vector<string> arr_temp = split_string(arr_temp_temp);

        vector<int> arr(n);

        for (int i = 0; i < n; i++) {
            int arr_item = stoi(arr_temp[i]);

            arr[i] = arr_item;
        }

        long result = countInversions(arr);

        fout << result << "\n";
    }

    fout.close();

    return 0;
}

vector<string> split_string(string input_string) {
    string::iterator new_end = unique(input_string.begin(), input_string.end(), [] (const char &x, const char &y) {
        return x == y and x == ' ';
    });

    input_string.erase(new_end, input_string.end());

    while (input_string[input_string.length() - 1] == ' ') {
        input_string.pop_back();
    }

    vector<string> splits;
    char delimiter = ' ';

    size_t i = 0;
    size_t pos = input_string.find(delimiter);

    while (pos != string::npos) {
        splits.push_back(input_string.substr(i, pos - i));

        i = pos + 1;
        pos = input_string.find(delimiter, i);
    }

    splits.push_back(input_string.substr(i, min(pos, input_string.length()) - i + 1));

    return splits;
}

#include <bits/stdc++.h>

using namespace std;

// node structure
struct node
{
   int *a;        // for store value
   node **pointer_of_child; // children pointer
   int n;            // number of stored value
   bool ident;          // for index node, ident=false & for a node, ident=true
   node *parent;     // for parent of node
};

node *root = nullptr;
int index_nodes_num = 0;
int leaf_nodes_num = 0;

// index node
node *init_I(int t)
{
   node *np = new node;
   np->a = new int[2 * t + 1];
   np->pointer_of_child = new node *[2 * t + 2];
   np->n = 0;
   np->ident = false;
   for (int i = 0; i < 2 * t + 2; i++) // for children
   {
      np->pointer_of_child[i] = nullptr;
   }
   np->parent = nullptr;
   index_nodes_num++;
   return np;
}

// leaf node
node *init_L(int d)
{
   node *np = new node;
   np->a = new int[2 * d];
   np->ident = true;
   np->pointer_of_child = new node *[2]; // for left or right sibling
   np->n = 0;
   for (int i = 0; i < 2; i++)
   {
      np->pointer_of_child[i] = nullptr;//pointer_of_child[0] = left sibling, pointer_of_child[1] = right sibling
   }
   np->parent = nullptr;
   leaf_nodes_num++;
   return np;
}

// split index node
void InsertionInBTree(node *curr, node *newptr, int separtor, int t)
{
   if (curr->parent == nullptr)
   {
      // here no parent node  so create node
      node *temp = init_I(t);
      temp->pointer_of_child[0] = curr;
      temp->pointer_of_child[1] = newptr;
      temp->a[0] = separtor;
      (temp->n)++;
      curr->parent = temp;
      newptr->parent = temp;
      root = temp;
      return;
   }
   else
   {
      curr = curr->parent;
      if (curr->n < 2 * t + 1)
      {
         // index node has space
         int count = (curr->n);
         while (curr->a[count - 1] > separtor and count > 0)
         {
            // a k liye
            curr->a[count] = curr->a[count - 1];
            // child pointers k liye
            curr->pointer_of_child[count + 1] = curr->pointer_of_child[count];
            count--;
         }
         (curr->n)++;
         curr->a[count] = separtor;
         curr->pointer_of_child[count + 1] = newptr;
         newptr->parent = curr;
         return;
      }
      else
      {
         // spilt index node
         node *newptr2 = init_I(t);
         if (curr->a[t - 1] < separtor and separtor < curr->a[t])//curr is the index node to be spiltted
         {
            for (int i = 0; i < t + 1; i++)
            {
               newptr2->a[i] = curr->a[t + i]; 
               newptr2->pointer_of_child[i + 1] = curr->pointer_of_child[t + i + 1];
               curr->pointer_of_child[t + i + 1] = nullptr;
               (newptr2->pointer_of_child[i + 1])->parent = newptr2;
            }
            curr->n = t;
            newptr2->n = t + 1;
            newptr2->pointer_of_child[0] = newptr;
            newptr->parent = newptr2;
            InsertionInBTree(curr, newptr2, separtor, t);
         }
         else if (separtor > curr->a[t])
         {
            int separtor2 = curr->a[t];//earlier it was t+1
            newptr2->pointer_of_child[0] = curr->pointer_of_child[t + 1];
            (newptr2->pointer_of_child[0])->parent = newptr2;
            for (int i = 1; i < t + 1; i++)
            {
               newptr2->a[i - 1] = curr->a[t + i];
               newptr2->pointer_of_child[i] = curr->pointer_of_child[t + i + 1];
               curr->pointer_of_child[t + i + 1] = nullptr;
               (newptr2->pointer_of_child[i])->parent = newptr2;
            }
            int count = t;
            while (newptr2->a[count-1] > separtor & count>0)
            {
               //finding correct position for the separator to be inserted
               // a k liye
               newptr2->a[count] = newptr2->a[count - 1];
               // child pointers k liye
               newptr2->pointer_of_child[count + 1] = newptr2->pointer_of_child[count];

               count--;
            }
            newptr2->n = t + 1;
            curr->n = t;
            newptr2->a[count] = separtor;
            newptr2->pointer_of_child[count + 1] = newptr;
            newptr->parent = newptr2;
            InsertionInBTree(curr, newptr2, separtor2, t);
         }
         else
         {
            int separtor2 = curr->a[t - 1];
            newptr2->pointer_of_child[0] = curr->pointer_of_child[t];
            (newptr2->pointer_of_child[0])->parent = newptr2;
            for (int i = 0; i < t + 1; i++)
            {
               newptr2->a[i] = curr->a[t + i];
               newptr2->pointer_of_child[i+1] = curr->pointer_of_child[t + i + 1];
               curr->pointer_of_child[t + i + 1] = nullptr;
               (newptr2->pointer_of_child[i+1])->parent = newptr2;
            }
            int count = t-1;
            while (newptr2->a[count-1] > separtor & count >0)
            {
               // a k liye
               curr->a[count] = curr->a[count - 1];
               // child pointers k liye
               curr->pointer_of_child[count + 1] = curr->pointer_of_child[count];

               count--;
            }
            newptr2->n = t + 1;
            curr->n = t;
            curr->a[count] = separtor;
            curr->pointer_of_child[count + 1] = newptr;
            newptr->parent = curr;
            InsertionInBTree(curr, newptr2, separtor2, t);
         }
      }
   }
}

void insert(int value, int d, int t)
{
   node *curr;
   curr = root;
   if (curr == nullptr)
   {
      // create new node
      root = init_L(d);
      root->a[0] = value;
      (root->n)++;
      // curr = root;
   }
   else
   {
      // Goes to leaf where value is to be stored
      while (curr->ident == false)
      {
         if (value < curr->a[0])
         {
            curr = curr->pointer_of_child[0];
         }
         else if (value >= curr->a[(curr->n) - 1])
         {
            curr = curr->pointer_of_child[curr->n];
         }
         else
         {
            for (int i = 1; i < curr->n; i++)
            {
               if (value >= (curr->a[i - 1]) && value < (curr->a[i]))
               {
                  curr = curr->pointer_of_child[i];
                  break;
               }
            }
         }
      }

      // if leaf node has space
      if (curr->n < 2 * d)
      {
         curr->a[curr->n] = value;
         (curr->n)++;
         sort(curr->a, curr->a + (curr->n));
      }
      else // split leaf node
      {
         int separtor;
         node *newptr = init_L(d);
         if (curr->a[d - 1] < value)
         {
            for (int i = 0; i < d; i++)
            {
                //moving a to new node(d values)
               newptr->a[i] = curr->a[d + i];
            }
            newptr->a[d] = value;
            curr->n = d; // reduced size after splitting
            newptr->n = d + 1;
            sort(newptr->a, newptr->a + (newptr->n));
            separtor = newptr->a[0];
         }
         else
         {
            for (int i = 0; i < d + 1; i++)
            {
               newptr->a[i] = curr->a[d - 1 + i];
            }
            curr->a[d - 1] = value;
            curr->n = d; // reduced size after splitting
            newptr->n = d + 1;
            sort(curr->a, curr->a + (curr->n));
            separtor = newptr->a[0];
         }
         node *right_sibling = curr->pointer_of_child[1];
         curr->pointer_of_child[1] = newptr;
         newptr->pointer_of_child[0] = curr;
         if (right_sibling != nullptr)
         {
            newptr->pointer_of_child[1] = right_sibling;
            right_sibling->pointer_of_child[0] = newptr;
         }
         // store separtor in index node
         InsertionInBTree(curr, newptr, separtor, t);//t is the parameter for index node
      }
   }
}

void display(int t, int d)
{
   node *curr;
   curr = root;
   cout << index_nodes_num << " " << leaf_nodes_num << " ";
   // while (curr->ident == false)
   // {
   //    curr = curr->pointer_of_child[0];
   // }
   // cout << "Root : ";
   for (int i = 0; i < root->n; i++)
   {
      cout << root->a[i] << " ";
   }
   // cout << endl;
   // cout << " Leaf values : ";
   // while (curr != nullptr)
   // {
   //    for (int i = 0; i < curr->n; i++)
   //    {
   //       cout << curr->a[i] << " ";
   //    }
   //    cout << " | ";
   //    curr = curr->pointer_of_child[1];
   // }
   cout << endl;
}

int main()
{
   int t, d;
   cin >> d >> t;
   int choice;
   while (1)
   {
      cin >> choice;
      if (choice == 1)
      {
         // Insert
         int key;
         cin >> key;
         insert(key, d, t);
      }
      else if (choice == 2)
      {
         // cout << index_nodes_num << " " << leaf_nodes_num << endl;
         display(t, d);
      }
   }
}

```c++
#include <iostream>

using namespace std;

typedef struct node_tag
{
    char value;
    struct node_tag *lchild;
    struct node_tag *rchild;
}node;


unsigned int g_left_subtree_max_dist = 0;
unsigned int g_right_subtree_max_distance = 0;

void get_max_depth_of_tree(node *root_node, unsigned int *depth)
{
    unsigned int left_subtree_depth, right_subtree_depth;

    if (root_node == NULL)
    {
        return;
    }

    if ((root_node->lchild != NULL) || (root_node->rchild != NULL))
    {
        (*depth)++;
    }

    left_subtree_depth = *depth;
    right_subtree_depth = *depth;

    if (root_node->lchild)
    {
        get_max_depth_of_tree(root_node->lchild, &left_subtree_depth);
    }

    if (root_node->rchild)
    {
        get_max_depth_of_tree(root_node->rchild, &right_subtree_depth);
    }

    *depth = (left_subtree_depth > right_subtree_depth) ? left_subtree_depth : right_subtree_depth;
}

unsigned g_max_distance = 0;
void search_max_distance_recursively(node *root_node)
{
    if (root_node == NULL)
    {
        return;
    }

    unsigned int left_depth = 0;
    unsigned int right_depth = 0;

    if (root_node->lchild)
    {
        left_depth++;
        get_max_depth_of_tree(root_node->lchild, &left_depth);
    }

    if (root_node->rchild)
    {
        right_depth++;
        get_max_depth_of_tree(root_node->rchild, &right_depth);
    }

    unsigned int distance = left_depth + right_depth;

    if (distance > g_max_distance)
    {
        g_max_distance = distance;
    }

    search_max_distance_recursively(root_node->lchild);
    search_max_distance_recursively(root_node->rchild);
}


int search_max_distance(node *root_node, unsigned int *length)
{
    if ((root_node == NULL) || (length == NULL))
    {
        return -1;
    }

    g_max_distance = 0;

    search_max_distance_recursively(root_node);

    *length = g_max_distance;

    return 0;
}

node *create_node(char value)
{
    node *new_node = (node*)malloc(sizeof(node));

    new_node->lchild = NULL;
    new_node->rchild = NULL;
    new_node->value = value;

    return new_node;
}


int main()
{
    node* node_a = create_node('A');
    node* node_b = create_node('B');
    node* node_c = create_node('C');
    node* node_d = create_node('D');
    node* node_e = create_node('E');
    node* node_f = create_node('F');
    node* node_g = create_node('G');
    node* node_h = create_node('H');
    node* node_i = create_node('I');
    node* node_j = create_node('J');
    node* node_k = create_node('K');

    node_a->lchild = node_b;
    node_a->rchild = node_c;

    node_b->lchild = node_d;
    node_b->rchild = node_e;
    //node_c->lchild = node_f;
    //node_c->rchild = node_g;

    //node_d->lchild = node_h;
    //node_f->rchild = node_i;

    node_d->rchild = node_f;
    node_e->lchild = node_g;

    node_f->lchild = node_k;
    node_g->lchild = node_i;
    node_g->rchild = node_h;

    node_h->lchild = node_j;

    unsigned int length = 0;
    int result;

    result = search_max_distance(node_a, &length);

    cout << "result is:" << result << endl << "length is:" << length << endl;

    return 0;
}

```

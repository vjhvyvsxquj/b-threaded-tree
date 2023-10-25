# b-threaded-tree
#include <stdio.h>
#include <stdlib.h>

struct TreeNode {
    int data;
    struct TreeNode* left;
    struct TreeNode* right;
    int isThreaded; // Flag to indicate whether the right pointer is threaded
};

// Function to create a new threaded binary tree node
struct TreeNode* createThreadedNode(int data) {
    struct TreeNode* newNode = (struct TreeNode*)malloc(sizeof(struct TreeNode));
    newNode->data = data;
    newNode->left = NULL;
    newNode->right = NULL;
    newNode->isThreaded = 0; // Initially, not threaded
    return newNode;
}

// Function to perform Morris Preorder Traversal
void morrisPreorderTraversal(struct TreeNode* root) {
    while (root) {
        if (root->left == NULL) {
            printf("%d ", root->data);
            root = root->right;
        } else {
            struct TreeNode* predecessor = root->left;
            while (predecessor->right && predecessor->right != root)
                predecessor = predecessor->right;
if (predecessor->right == NULL) {
                printf("%d ", root->data);
                predecessor->right = root;
                root = root->left;
            } else {
                predecessor->right = NULL;
                root = root->right;
            }
        }
    }
}

// Function to perform Morris Preorder Traversal on a threaded binary tree
void threadedMorrisPreorderTraversal(struct TreeNode* root) {
    if (root == NULL) return;
    
    struct TreeNode* current = root;
    while (current) {
        if (current->left == NULL) {
            printf("%d ", current->data);
            current = current->right;
        } else {
            struct TreeNode* predecessor = current->left;
            while (predecessor->right && predecessor->right != current)
                predecessor = predecessor->right;

            if (predecessor->right == NULL) {
                printf("%d ", current->data);
                predecessor->right = current;
current = current->left;
            } else {
                predecessor->right = NULL;
                current = current->right;
            }
        }
    }
}

int main() {
    struct TreeNode* root = createThreadedNode(1);
    root->left = createThreadedNode(2);
    root->right = createThreadedNode(3);
    root->left->left = createThreadedNode(4);
    root->left->right = createThreadedNode(5);

    printf("Morris Preorder Traversal: ");
    morrisPreorderTraversal(root);

    printf("\nThreaded Morris Preorder Traversal: ");
    threadedMorrisPreorderTraversal(root);

    return 1;
}

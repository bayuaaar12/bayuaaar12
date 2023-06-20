
//bayu anugrah ramadan
#include <stdio.h>
#include <stdlib.h>

#define MAX 3
#define MIN 2

struct BTreeNode{
	char data[MAX + 1];
	int count;
	struct BTreeNode *link[MIN + 1];
};

struct BTreeNode *root;

struct BTreeNode *createNode(char data, struct BTreeNode *child) {
	struct BTreeNode *newNode;
	newNode = (struct BTreeNode *)malloc(sizeof(struct BTreeNode));
	newNode->data[1] = data;
	newNode->count = 1;
	newNode->link[0] = root;
	newNode->link[1] = child;
	return newNode;
}

void insertNode(char data, int pos, struct BTreeNode *node, struct BTreeNode *child){
	int j = node->count;
	
	while(j > pos){
		node->data[j + 1] = node->data[j];
		node->link[j + 1] = node->link[j];
		j--;
	}
	
	node->data[j + 1] = data;
	node->link[j + 1] = child;
	node->count++;
}

void splitNode(char data, char *pval, int pos, struct BTreeNode *node, struct BTreeNode *child, struct BTreeNode **newNode){
	int median, j;
	
	if(pos > MIN){
		median = MIN + 1;
	}
	else{
		median = MIN;
	}	
	
	*newNode = (struct BTreeNode *)malloc(sizeof(struct BTreeNode));
	j = median + 1;
	
	while (j <= MAX){
		(*newNode)->data[j - median] = node->data[j];
		(*newNode)->link[j - median] = node->link[j];
		j++;
	}
	
	node->count = median;
	(*newNode)->count = MAX - median;
	
	if (pos <= MIN){
		insertNode(data, pos, node, child);
	}
	else{
		insertNode(data, pos - median, *newNode, child);
	}
	
	*pval = node->data[node->count];
	(*newNode)->link[0] = node->link[node->count];
	node->count--;
}

int setValue(char data, char *pval,struct BTreeNode *node, struct BTreeNode **child){
	int pos;
	
	if (!node){
		*pval = data;
		*child = NULL;
		return 1;
	}
	
	if (data < node->data[1]){
		pos = 0;
	}
	else {
		for(pos = node->count; (data < node->data[pos] && pos > 1); pos--){
			if(data == node->data[pos]) {
				printf("Duplicates are not permitted\n");
				return 0;
			}
		}
	}
	
	if(setValue(data, pval, node->link[pos], child)){
		if(node->count < MAX){
			insertNode(*pval, pos, node, *child);
		} 
		else{
			splitNode(*pval, pval, pos, node, *child, child);
			return 1;
		}
	}
	return 0;
}

void insert(char data) {
	int flag;
	char i;
	struct BTreeNode *child;
	
	flag = setValue(data, &i, root, &child);
	if(flag){
		root = createNode(i, child);
	}
}

void traversal(struct BTreeNode *myNode) {
	int i;
	if (myNode) {
	for (i = 0; i < myNode->count; i++) {
		traversal(myNode->link[i]);
		printf("%c ", myNode->data[i + 1]);
	}
	traversal(myNode->link[i]);
	}
}

int main() {
	char data, ch;
	
	insert('E');
	insert('I');
	insert('L');
	insert('G');
	insert('K');

	
	traversal(root);
	
	printf("\n");
}

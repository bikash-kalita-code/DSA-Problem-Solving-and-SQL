# Circular Linked List

### Code

#### C

```c
#include <stdio.h>
#include <stdlib.h>

struct node
{
	int info;
	struct node *link;
};

struct node *create_list(struct node *last);
void display(struct node *last);
struct node *addtoempty(struct node *last, int data);
struct node *addatbeg(struct node *last, int data);
struct node *addatend(struct node *last, int data);
struct node *addafter(struct node *last, int data, int item);
struct node *del(struct node *last, int data);

int main()
{
	int choice, data, item;
	struct node *last = NULL;
	while (1)
	{
		printf("1. Create List\n");
		printf("2. Display\n");
		printf("3. Add to empty\n");
		printf("4. Add at beginning\n");
		printf("5. Add at end\n");
		printf("6. Add after\n");
		printf("7. Delete\n");
		printf("8. Quit\n");
		printf("Enter your choice : \n");
		scanf("%d", &choice);
		switch (choice)
		{
		case 1:
			last = create_list(last);
			break;
		case 2:
			display(last);
			break;
		case 3:
			printf("Enter the element to be inserted : \n");
			scanf("%d", &data);
			last = addtoempty(last, data);
			break;
		case 4:
			printf("Enter the element to be inserted : \n");
			scanf("%d", &data);
			last = addatbeg(last, data);
			break;
		case 5:
			printf("Enter the element to be inserted : \n");
			scanf("%d", &data);
			last = addatend(last, data);
			break;
		case 6:
			printf("Enter the element to be inserted : \n");
			scanf("%d", &data);
			printf("Enter the element after which to insert : \n");
			scanf("%d", &item);
			last = addafter(last, data, item);
			break;
		case 7:
			printf("Enter the element to be deleted : \n");
			scanf("%d", &data);
			last = del(last, data);
			break;
		case 8:
			exit(1);
		default:
			printf("Wrong choice !\n");
		}
	}
	return 0;
}

struct node *create_list(struct node *last)
{
	int n, data;
	printf("Enter number of elements to be created :\n");
	scanf("%d", &n);
	if (n == 0)
		return last;
	printf("Enter the element to be inserted : \n");
	scanf("%d", &data);
	last = addtoempty(last, data);
	for (int i = 2; i <= n; i++)
	{
		printf("Enter the element to be inserted : \n");
		scanf("%d", &data);
		last = addatend(last, data);
	}
	return last;
}

void display(struct node *last)
{
	struct node *p;
	if (last == NULL)
	{
		printf("List is empty\n");
		return;
	}
	p = last->link;
	printf("List elements are :\n");
	do
	{
		printf("%d\t", p->info);
		p = p->link;
	} while (p != last->link);
	printf("\n");
}

struct node *addtoempty(struct node *last, int data)
{
	struct node *tmp = (struct node *)malloc(sizeof(struct node));
	tmp->info = data;
	last = tmp;
	tmp->link = last;
	return last;
}

struct node *addatbeg(struct node *last, int data)
{
	struct node *tmp;
	tmp = (struct node *)malloc(sizeof(struct node));
	tmp->info = data;
	tmp->link = last->link;
	last->link = tmp;
	return last;
}

struct node *addatend(struct node *last, int data)
{
	struct node *tmp = (struct node *)malloc(sizeof(struct node));
	tmp->info = data;
	tmp->link = last->link;
	last->link = tmp;
	last = tmp;
	return last;
}

struct node *addafter(struct node *last, int data, int item)
{
	struct node *tmp = (struct node *)malloc(sizeof(struct node));
	tmp->info = data;
	if (last->info == item && last->link == last)
	{
		tmp->link = last->link;
		last->link = tmp;
		last = tmp;
		return last;
	}
	struct node *p = last->link;
	while (p != last)
	{
		if (p->info == item)
		{
			tmp->link = p->link;
			p->link = tmp;
			if (p == last)
				last = tmp;
			return last;
		}
		p = p->link;
	}
	if (p == last && p->info == item)
	{
		tmp->link = p->link;
		p->link = tmp;
		last = tmp;
		return last;
	}
	printf("Element %d not present in the list\n", item);
	return last;
}

struct node *del(struct node *last, int data)
{
	struct node *tmp, *p;
	if (last == NULL)
	{
		printf("List is empty\n");
		return last;
	}
	// Deletion of only node
	if (last->link == last && last->info == data)
	{
		tmp = last;
		last = NULL;
		free(tmp);
		return last;
	}
	// Deletion of the first node
	if (last->link->info == data)
	{
		tmp = last->link;
		last->link = tmp->link;
		free(tmp);
		return last;
	}
	// Deletion in between
	p = last->link;
	while (p->link != last)
	{
		if (p->link->info == data)
		{
			tmp = p->link;
			p->link = tmp->link;
			free(tmp);
			return last;
		}
		p = p->link;
	}
	// Deletion of last node
	if (last->info == data)
	{
		tmp = last;
		p->link = last->link;
		last = p;
		free(tmp);
		return last;
	}
	printf("Element %d not found in the list\n", data);
	return last;
}
```
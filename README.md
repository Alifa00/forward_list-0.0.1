```
#include <iostream>
#include <sstream>
using namespace std;

struct node_t {
	int value;
	node_t * next;
};

bool input(char &op, int &num)
{
	string str;
	char excess, sym;
	int n;
	getline(cin, str);
	istringstream stream(str);
	if (stream >> sym && (sym == '+' || sym == '-' || sym == '/' || sym == '=' || sym == 'q'))
	{
		if (sym == '+')
		{
			if ((stream >> n) && !(stream >> excess))
			{
				num = n;
				op = sym;
				return true;
			}
			else
			{
				return false;
			}
		}


		if (!(stream >> excess))
		{
			op = sym;
			return true;
		}
		else
		{
			return false;
		}
	}
	else
	{
		return false;
	}
}

void output(node_t *b)
{
	node_t *print = b;
	int counter = 0;

	while (print)
	{
		counter++;
		print = print->next;
	}
	print = b;

	for (int i = 0; i < counter; i++)
	{
		cout << "+---+    ";
	}
	cout << '\n';

	for (int i = 0; i < counter; i++)
	{
		cout << "| " << print->value << " |";
		print = print->next;
		if (print) cout << "--->";
	}
	cout << '\n';

	for (int i = 0; i < counter; i++)
	{
		cout << "+---+    ";
	}
	cout << '\n';
}

void add(node_t *&begin, node_t *&end, int num)
{
	end->next = new node_t;
	end = end->next;
	end->value = num;
	end->next = NULL;

	if (begin->next == NULL)
	{
		begin->next = end;
	}
} 

void del(node_t *&begin)
{
	if (begin == NULL) return;

	node_t *tmp = begin;
	begin = tmp->next;
	delete tmp;

}

void reverse(node_t *&begin, node_t *&end)
{
	if (begin == NULL) return;
	node_t * curr, *next, *prev = NULL;
	end = begin;
	curr = begin;
	while (curr)
	{
		next = curr->next;
		curr->next = prev;
		prev = curr;
		curr = next;
	}
	begin = prev;
}

void del_node(node_t *&begin)
{
	if (begin == NULL) return;

	node_t *p = begin;
	node_t *t;

	while (p)
	{
		t = p;
		p = p->next;
		delete t;
	}
	begin = NULL;
}

int main()
{
	node_t *begin = NULL, *end = NULL;
	char op = ' ';
	int num;

	while (op != 'q')
	{
		if (!input(op, num))
		{
			cout << "An error has occured while reading input data.\n";
			continue;
		}

		switch (op)
		{
			case '+':
				if (!begin)
				{
					begin = new node_t;
					begin->value = num;
					begin->next = NULL;
					end = begin;
				}
				else
				{
					add(begin, end, num);
				}
				output(begin);
				break;

			case '-':
				del(begin);
				output(begin);
				break;

			case '/':
				reverse(begin, end);
				output(begin);
				break;
			
			case '=':
				output(begin);
				break;
		}
	}
	del_node(begin);
    return 0;
}


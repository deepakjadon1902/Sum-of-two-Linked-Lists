import java.util.Scanner;

class Node {
    int data;
    Node next;

    Node(int d) {
        data = d;
        next = null;
    }
}

class LinkedList {
    Node head;

    public LinkedList() {
        head = null;
    }

    // Function to insert a node at the end of the linked list
    public void insert(int data) {
        Node newNode = new Node(data);
        if (head == null) {
            head = newNode;
        } else {
            Node temp = head;
            while (temp.next != null) {
                temp = temp.next;
            }
            temp.next = newNode;
        }
    }
}

public class Main {
    static int carry = 0;

    public static Node addLists(Node first, Node second) {
        int len1 = length(first);
        int len2 = length(second);

        // Pad the shorter linked list with zeros
        if (len1 < len2) {
            first = padZeros(first, len2 - len1);
        } else if (len2 < len1) {
            second = padZeros(second, len1 - len2);
        }

        // Add the lists
        Node sum = addListsHelper(first, second);

        // If there's a carry after adding the lists, create a new node for it
        if (carry > 0) {
            Node carryNode = new Node(carry);
            carryNode.next = sum;
            sum = carryNode;
        }

        return sum;
    }

    public static Node addListsHelper(Node first, Node second) {
        if (first == null && second == null)
            return null;

        Node result = new Node(0);

        // Recursive call to next nodes
        result.next = addListsHelper(first.next, second.next);

        // Add the current nodes' data and the carry
        int sum = first.data + second.data + carry;
        result.data = sum % 10; // Update result data
        carry = sum / 10; // Update carry for next calculation

        return result;
    }

    // Function to pad the shorter linked list with zeros
    public static Node padZeros(Node head, int diff) {
        Node temp = head;
        while (diff-- > 0) {
            Node newNode = new Node(0);
            newNode.next = temp;
            temp = newNode;
        }
        return temp;
    }

    // Function to calculate the length of the linked list
    public static int length(Node head) {
        int len = 0;
        Node temp = head;
        while (temp != null) {
            len++;
            temp = temp.next;
        }
        return len;
    }

    // Function to print the linked list
    public static void printList(Node head) {
        Node temp = head;
        while (temp != null) {
            System.out.print(temp.data + " ");
            temp = temp.next;
        }
    }

    public static void main(String args[]) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        int m = scanner.nextInt();

        LinkedList list1 = new LinkedList();
        for (int i = 0; i < n; i++) {
            list1.insert(scanner.nextInt());
        }

        LinkedList list2 = new LinkedList();
        for (int i = 0; i < m; i++) {
            list2.insert(scanner.nextInt());
        }

        Node sumListHead = addLists(list1.head, list2.head);
        printList(sumListHead);

        scanner.close();
    }
}

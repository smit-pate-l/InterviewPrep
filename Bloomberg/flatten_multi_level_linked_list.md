## Flatten a multi level doubly linked list

You are given a doubly linked list, which contains nodes that have a next pointer, a previous pointer, and an additional child pointer. This child pointer may or may not point to a separate doubly linked list, also containing these special nodes. These child lists may have one or more children of their own, and so on, to produce a multilevel data structure as shown in the example below.

Given the head of the first level of the list, flatten the list so that all the nodes appear in a single-level, doubly linked list. Let curr be a node with a child list. The nodes in the child list should appear after curr and before curr.next in the flattened list.

Return the head of the flattened list. The nodes in the list must have all of their child pointers set to null.

### Recurrsive Approach

    def flatten(head):
        if not head:
            return head

        # pseudo head to ensure we never have prev == None
        pseudoHead = Node(None, None, head, None)
        dfs(pseudoHead, head)

        # detach pseudoHead from linked list
        pseudoHead.next.prev = None
        return pseudoHead.next

    def dfs(prev, curr):

    if curr is None:
        return prev

    curr.prev = prev
    prev.next = curr

    # save curr.next since, it could be tempered in recursive call
    tempNext = curr.next

    # left sub-tree rec. call
    tail = dfs(curr, curr.child)
    curr.child = None

    # right sub-tree rec. call with tail as prev
    return dfs(tail, tempNext)

### Iterative Approach

    def flatten(head):
        if not head:
            return head

        pseudoHead = Node(None, None, head, None)
        prev = pseudoHead

        stack = []
        stack.append(head)

        while stack:

            curr = stack.pop()

            curr.prev = prev
            prev.next = curr

            # append right child to stack
            if curr.next:
                stack.append(curr.next)

            # append left child to stack
            if curr.child:
                stack.append(curr.child)
                # don't forget to remove all child pointers.
                curr.child = None

            prev = curr
        pseudoHead.next.prev = None
        return pseudoHead.next

Time Complexity: **O(N)**
Space Complexity: **O(N)**

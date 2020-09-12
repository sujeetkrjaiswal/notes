# Linkedlist

## 1: For a singly linked list, check if its a palindrome without modifying the list with O\(1\) space complexity and O\(n\) time complexity

```javascript
class ListNode {
  constructor(value, next = null) {
    this.value = value;
    this.next = next;
  }
}

function isListPalindrome(l) {
  if (l === null) return true; // for cases []
  const start = l;
  const [midPrev, mid] = getMiddleNode(l);
  if (midPrev === null) return true; // for cases [1]
  const reversed = reverseList(mid);
  let isPalindrome = true;
  let sn = start;
  let mn = reversed;
  midPrev.next = null;
  while (sn && mn) {
    if (sn.value !== mn.value) {
      isPalindrome = false;
      break;
    }
    sn = sn.next;
    mn = mn.next;
  }
  const backReversed = reverseList(reversed);
  midPrev.next = backReversed;
  return isPalindrome;
}
function getMiddleNode(l) {
  let fp = l;
  let sp = l;
  let midPrev = null;
  while (fp !== null && fp.next !== null) {
    fp = fp.next.next;
    midPrev = sp;
    sp = sp.next;
  }
  return [midPrev, sp];
}
function reverseList(l) {
  let node = l;
  let prev = null;
  let next = null;
  while (node) {
    next = node.next;
    node.next = prev;
    prev = node;
    node = next;
  }
  return prev;
}
function getArr(l) {
  const a = [];
  let node = l;
  while (node) {
    a.push(node.value);
    node = node.next;
  }
  return a;
}

const l = new ListNode(3, new ListNode(5, new ListNode(3, new ListNode(5))));
console.log(isListPalindrome(l));
```


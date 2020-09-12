# Activity Selection

## Problem statement

Given a list of activity \(name, start time, finish time\), select maximum activities that can be performed by a user, assuming only one activiy can be done at a time

### Example

For below given Activity schedule, select maximum activity that can be performed by the user.

| Activity | A1 | A2 | A3 | A4 | A5 | A6 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Start | 0 | 4 | 1 | 5 | 5 | 8 |
| Finish | 6 | 4 | 2 | 8 | 7 | 9 |

**Output** 4 \(A3, A2, A5, A6\)

**Pseudo code: \(This is based on greedy algorithm\): O\(n.log\_n\)**

1. Sort the activiy based on the finish time
2. selected\_activity = \[first activity\]
3. loop over activitoes for i = 1 to n-2:

   if currentActivity.startTime &gt;= lastActivity.finishTime  
   selected\_activity.push\(currentActivity\)

{% code title="acitivity\_selection.ts" %}
```typescript
type Activity = {
  name: string;
  startTime: number;
  endTime: number;
};


function selectActivity(activities: Activity[]): Activity[] {
  if(activities.length === 0) return []
  const selectedActivities: Activity[] = []
  activities.sort((a,b) => a.endTime - b.endTime)
  const iter = activities[Symbol.iterator]()
  selectedActivities.push(iter.next().value)
  while(true) {
    const current = iter.next()
    if(current.done) {
      break
    }
    if(current.value.startTime >= selectedActivities[selectedActivities.length - 1].endTime) {
      selectedActivities.push(current.value)
    }
  }
  return selectedActivities
}


const sample: Activity[] = [
  {
    name: "A1",
    startTime: 0,
    endTime: 6,
  },
  {
    name: "A2",
    startTime: 3,
    endTime: 4,
  },
  {
    name: "A3",
    startTime: 1,
    endTime: 2,
  },
  {
    name: "A4",
    startTime: 5,
    endTime: 8,
  },
  {
    name: "A5",
    startTime: 5,
    endTime: 7,
  },
  {
    name: "A6",
    startTime: 8,
    endTime: 9,
  },
];
console.table(selectActivity(sample));
```
{% endcode %}


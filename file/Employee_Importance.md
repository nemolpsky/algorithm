## Employee Importance

You are given a data structure of employee information, which includes the employee's unique id, his importance value and his direct subordinates' id.

For example, employee 1 is the leader of employee 2, and employee 2 is the leader of employee 3. They have importance value 15, 10 and 5, respectively. Then employee 1 has a data structure like [1, 15, [2]], and employee 2 has [2, 10, [3]], and employee 3 has [3, 5, []]. Note that although employee 3 is also a subordinate of employee 1, the relationship is not direct.

Now given the employee information of a company, and an employee id, you need to return the total importance value of this employee and all his subordinates.

- Example 1:

  Input: [[1, 5, [2, 3]], [2, 3, []], [3, 3, []]], 1

  Output: 11

  Explanation:

  Employee 1 has importance value 5, and he has two direct subordinates: employee 2 and employee 3. They both have importance value 3. So the total importance value of employee 1 is 5 + 3 + 3 = 11.
 

- Note:

  - One employee has at most one direct leader and may have several subordinates.
  - The maximum number of employees won't exceed 2000.

---

## 解题思路
这道题是给定了一个集合，里面存储了Employee对象，有id、importance和subordinates字段，分别是id、重要程度值和下属，现在要求给定一个id计算出这个id和其下属所有的重要程度值之和。

- 排序遍历查找

  首先把整个集合按id排序，然后找到起始id和将它的下属放入map，依次遍历的时候如果发现在map中找到就表示是其下属，同时把二级下属也放入map中，最后遍历完就把所有的下属链都计算好了，时间复杂度是O(nlogn)。

  ```

	public static int getImportance(List<Employee> employees, int id) {

		Collections.sort(employees, new Comparator<Employee>() {
			@Override
			public int compare(Employee o1, Employee o2) {
				return o1.id - o2.id;
			}
		});

		if (employees.isEmpty()) {
			return 0;
		}

		int num = 0;

		HashMap<Integer, Integer> map = null;
		// 遍历集合
		for (Employee e : employees) {
			// map中有存，累加值，把下属放入map中
			if (map != null && map.get(e.id) != null) {
				for (int i : e.subordinates) {
					map.put(i, i);
				}
				num += e.importance;
			}

			// 找到起始id，累加值，把下属放入map中
			if (e.id == id) {
				num += e.importance;
				map = new HashMap<>();
				for (int i : e.subordinates) {
					map.put(i, i);
				}
			}
		}

		return num;
	}
  ```

- 深度优先搜索

  上面的方法因为要排序所以性能小号比较大，如果数据是已经排好序了那就会是个很好的选择，但是没排序比较适合先用map保存数据结构，再进行深度优先搜索，时间复杂度是O(n)。

  ```
	static HashMap<Integer, Employee> map = null;

	public static int getImportance1(List<Employee> employees, int id) {

		if (employees.isEmpty()) {
			return 0;
		}
		
		// 存储整个数据结构
		map = new HashMap<>();
		for (Employee employee : employees) {
			map.put(employee.id, employee);
		}

		return search(id, 0);
	}

	public static int search(int id, int num) {
		Employee employee = map.get(id);
		// 找到起始id，累加值
		if (employee != null) {
			num += employee.importance;

			// 有下属的情况下，递归查找累加
			List<Integer> subordinates = employee.subordinates;
			if (subordinates != null && !subordinates.isEmpty()) {
				for (Integer cId : subordinates) {
					num += search(cId, num);
				}
			}
		}

		return num;
	}

  ```
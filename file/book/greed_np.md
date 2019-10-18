## NP完全问题

NP完全问题的英文名称是Non-deterministic Polynomial的问题，即多项式复杂程度的非确定性问题。

抛开那些复杂的定义简单来说就是没有任何一个够快的方法可以在合理的时间内找到答案。只能使用穷穷举的方式来尝试所有的方法，并且子集越多时间耗费的时间也会成倍增长。

- 旅行商问题
 
  可以看到一个人要经过所有的城市，并且要找出最近的路线，那可以从任意一个城市出发，只要经过所有的城市就行了，会发现随着城市数量增多，可以选择的路线也越来越多，时间复杂度是O(n!)，也就是阶乘，这就是一个NP完全问题。

---

## 贪婪算法 

顾名思义，贪婪算法的核心就在于每一步都是选择最优解，既然局部每次都是最优解，那最终解即使不是最优的也不会相差太多，因此贪婪算法也是一种近似算法。一般来讲贪婪算法的步骤都比较简单，运行时间也不会太慢，通常是根据速度和结果与最优解的差别来判断一个贪婪算法的好坏。

- 覆盖问题

  比如假设有一个集合，里面包含了许多不同的州，然后还有很多子集合，里面包含的是一个广播站可以覆盖的州，现在需要找到能够覆盖所有州的广播站组合。这个时候就很适合用贪婪算法。

  可以看到下面的代码，每次循环都找出覆盖州最多的一个广播站，然后将已经被覆盖的州移除集合，继续对那些未被覆盖的州寻找覆盖最多的广播站，知道所有的州都被覆盖，时间复杂度仅仅是O(n)，n是广播站的个数。

  ```
	public static void greed() {
		// 需要覆盖的州
		Set<String> statesNeeded = new HashSet(Arrays.asList("mt", "wa", "or", "id", "nv", "ut", "ca", "az"));

		// 不同的广播站集合
		Map<String, Set<String>> stations = new LinkedHashMap<>();

		stations.put("kone", new HashSet<>(Arrays.asList("id", "nv", "ut")));
		stations.put("ktwo", new HashSet<>(Arrays.asList("wa", "id", "mt")));
		stations.put("kthree", new HashSet<>(Arrays.asList("or", "nv", "ca")));
		stations.put("kfour", new HashSet<>(Arrays.asList("nv", "ut")));
		stations.put("kfive", new HashSet<>(Arrays.asList("ca", "az")));

		// 最终选择的出来的覆盖所有州的集合
		Set<String> finalStations = new HashSet<String>();

		// 只要还有元素没覆盖到就一直循环
		while (!statesNeeded.isEmpty()) {

			// 最优选择的广播站
			String bestStation = null;
			// 存放被覆盖的州
			Set<String> statesCovered = new HashSet<>();

			// 遍历所有可选集合找出最优的覆盖
			for (Map.Entry<String, Set<String>> station : stations.entrySet()) {
				// 将需要覆盖的元素放入集合
				Set<String> covered = new HashSet<>(statesNeeded);
				// 求交集，也就是该广播站覆盖的州
				covered.retainAll(station.getValue());
				// 如果交集的长度比已经被覆盖的范围大就表示是更优的选择，就替换key和被替换的元素
				if (covered.size() > statesCovered.size()) {
					bestStation = station.getKey();
					statesCovered = covered;
				}
				// 移除已经覆盖的元素
				statesNeeded.removeIf(statesCovered::contains);
				// 找到最优覆盖的key放入集合中
				if (bestStation != null) {
					finalStations.add(bestStation);
				}
			}
		}

		// [ktwo, kone, kthree, kfive]
		System.out.println(finalStations);
	}
  ```

---

## 总结
1. NP完全问题

   上面讲了NP其实就是一些特别复杂只能用穷举来一个个验证的问题，大致上符合了下面几点。

   - 元素较少时算法的运行速度非常快，但随着元素数量的增加，速度会变得非常慢。
   - 涉及“所有组合”的问题通常是NP完全问题。
   - 不能将问题分成小问题，必须考虑各种可能的情况。这可能是NP完全问题。
   - 如果问题涉及序列（如旅行商问题中的城市序列）且难以解决，它可能就是NP完全问题。
   - 如果问题涉及集合（如广播台集合）且难以解决，它可能就是NP完全问题。
   - 如果问题可转换为集合覆盖问题或旅行商问题，那它肯定是NP完全问题。

2. 贪婪算法
   
   按照上面的介绍，贪婪算法其实就是每一步都取得当前的最优解，来期望最终解也是最优解，但是事实上并不能完全保证是最优解，只是在一些特定情况下可能是最优解。比如下列算法就是贪婪算法，在特定情况下可以得到最优解。

   - 广度优先搜索算法
   - 狄克斯特拉算法
     

   
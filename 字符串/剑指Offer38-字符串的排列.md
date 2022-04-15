[剑指 Offer 38. 字符串的排列](https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/)

```
剑指 Offer 38. 字符串的排列
输入一个字符串，打印出该字符串中字符的所有排列。
你可以以任意顺序返回这个字符串数组，但里面不能有重复元素。

示例:
输入：s = "abc"
输出：["abc","acb","bac","bca","cab","cba"]
```

```
思路：https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/solution/mian-shi-ti-38-zi-fu-chuan-de-pai-lie-hui-su-fa-by/

回溯法
思想：
1、使用深度优先算法，针对存在重复字符的字符串使用剪枝优化去除
2、对于一个不含重复字符的字符串来说，共有 n! 种排列组合
3、对于abc字符串来说
确定第一位为a或者b或者c，然后递归确定第二位为b或者c，然后确定第三位为c，当确定到末尾时，本地排列完成，将组合放入结果。
关键在于递归完毕时，将调整顺序的字符需要调整回来！
```

```go

// 字符串s转译为字符切片
var base []byte

// 存储所有结果集
var res []string

func permutation(s string) []string {
	//初始化
	base = make([]byte, 0)
	res = make([]string, 0)

	// 字符串s转译为字符切片
	base = []byte(s)
	dfs(0)
	return res
}

// dfs 使用深度优先算法对字符串求穷集
// 递归终结条件：当遍历到最后一位字符的时候
// 深度优先算法优化：剪枝，即将每次递归时出现的重复的字符跳过，abb这种
func dfs(x int) {
	// 首先判断递归终结条件
	if x == len(base)-1 {
		// 到了最后一位，则不需要再递归，将此时的base转译为字符串存储进res中
		res = append(res, string(base))
		return
	}

	// 判断剪枝map
	var m = make(map[string]int)

	// 开始固定每个字符的位置进行处理，初始x=0开始
	for i := x; i < len(base); i++ {
		// 首先判断是否需要剪枝
		if _, ok := m[string(base[i])]; ok {
			continue
		}
		// 将字符存入m中
		m[string(base[i])] = 1

		// 交换i，x位置的字符，固定base[i]的字符
		// 先固定base[0]的字符，然后递归固定base[1]的字符，....
		// abc: 即第一次base[0]=a，第二次base[0]=b，第二次base[0]=c
		swap(i, x)

		// 递归确定base[x+1]的位置，即开始获取x后面的位置的字符
		dfs(x + 1)

		// 还原base字符的位置为原始位置，以方便接下来的迭代
		swap(i, x)
	}
}

func swap(i, x int) {
	tmp := base[i]
	base[i] = base[x]
	base[x] = tmp
}
```
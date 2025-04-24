# Leetcode 分类顺序表第二版(增加重点250题)

# 1.1. 前言

这本书是根据我个人的理解，对Leetcode（[https://leetcode.com/）](https://leetcode.com/%EF%BC%89) 这个网站中的算法题目重新进行分类划分，并以**先易后难**的顺序重新进行排序。旨在为大家节省时间，可以更好更有效率的做题。

# 1.1.1. 背景

在如今的计算机行业领域，无论是在校学生参加计算机算法编程竞赛ACM，还是对于求职找工作的毕业生，甚至在职跳槽的计算机从业工作者，都逃不开“刷题”这座大山。尤其是在北美，Google，Facebook，Microsoft，Amazon 等等大公司，无一不考刷题，以算法面试为主。而无论是北美留学生，还是工作几年的上班族，想进大公司，唯一的出路就是刷题。同样，在国内，也已经逐渐向北美靠近，很多大公司也在招人面试中加入算法面试。而Leetcode这个算法题目网站，已经是目前为止最贴近大公司算法面试题目的网站。全世界的学生，在职人员，都以Leetcode为主，无论每个人的初衷是什么，我们都要在此挑战一件事 — — 刷题！

# 1.1.2. 关于我

我自己在16年初来到美国，本科是Software Engineering，研究生是Computer Science。在北美留学两年时间里，除了上课可以说刷题这件事陪伴了我整个两年多的留学生涯。16年秋季找实习17年秋季找全职，从15年前辈那里知道了CS专业的当年形式一片大好，到自己亲历16年的急转直下：各大公司提前招满，有的甚至12月不到就已经停止招人，17年更是每况愈下。但即使形式再差，CS也依旧算是北美第一专业。

所以整个北美对于刷题一事，是人人逃不过的大山，但现在来说，刷题最重要的资源Leetcode网站，并没有一个非常好的分类顺序。Leetcode本身的分类做的的确不错，但对于题目的顺序并没有给出，而且有的分类并不是非常好。所以我根据我自己的刷题经验总结，对Leetcode这个网站的题目重新进行分类划分，并按照先易后难的顺序，针对每个分类下面，进行排序。旨在让从来没有刷过题的人，可以有一个系统性的参考。

我个人本身两年来刷题3000多，对Leetcode中大多数题目可以说了如指掌，并且把Leetcode 800道题每道题，进行一题多解 + 最优解，思路解法步骤，时间空间复杂度的讲解，录成视频（现以更新完前400道）。我自己创业，现已经有了自己的团队，也有各种 Google Facebook 等诸多大公司的人员加入。也曾有像是Leetcode的员工，在我的公众号问我是否想加入Leetcode团队。所以我想通过我自己对于题目的理解，通过我们的努力，给大家提供一个刷Leetcode的解决方案。

# 1.1.3. 致所有本书使用者

本书以先易后难 + 分类而成。如果是第一次刷题的小伙伴，最好以本书的顺序为主，可以减少很多刷题的负担。对于类别的先后顺序我并没有给，这个我自己正在总结。因为现阶段我自己开算法直播课为一部分人讲解北美算法基础 + 面试，也在自己做这个对于分类的顺序，所以会在第二版或者第三版出这个顺序。现在的这个顺序是包括了Leetcode前400题（除去16到SQL题目）。

然后很多小伙伴问我刷400道够不够的问题，我的回答是完全够的，只要能每一道题理解到位，400题已经足以应付公司面试题。Leetcode在16年末也仅仅只有400题，800是近一年才涨出来的。做完400道基本就需要3个月的时间了，慢一点4个月也是有可能的。所以我建议大家不要多刷，精刷 + 总结更重要。

# 1.1.4. 网站

如果有小伙伴想看我录的前400道题目的解法视频，请到Cspiration（[https://cspiration.com/）](https://cspiration.com/%EF%BC%89) 购买观看，我还会持续更新把后400题更新完毕，并会出更多的课程帮助大家。

# 1.1.5. 北美刷题找工实习微信群

如果你是一个人刷题想认识更多来自五湖四海的小伙伴，我们有免费的微信群可以加入，旨在让每个人不是孤军奋战，同胞间相互帮助分享资源，为大家排忧解难。当然，如果你对刷题或者找工有什么疑问，出现瓶颈，可以咨询我，如果我能帮到的我都会尽量给解答。

个人微信号：cspiration（加我拉你入群～）

微信公众号：cspiration（没错，和个人微信一样的～）

本书完全是由我个人编写而成，所以文中所有讲解，分类等，我尽量以客观公正的态度写。而且这个版本我会不断更新，局部很多会在以后有改动，请大家敬请期待。如有意见和建议，请微信或者邮箱联系我：[admin@cspiration.com](https://cspiration.com/leetcodeClassificationmailto:admin@cspiration.com)

2018.6.19

Edward Shi

# 1.2. Leetcode 题目视频讲解

所有题目进行视频讲解，刷题速度至少提升2–3倍。最优解 + 一题多解 + Test Case + 时间／空间复杂度 <https://cspiration.com/course/leetcodeVideo>

# 1.3. Leetcode 分类顺序

# 1.3.1. Array

**基础**27Remove Element26Remove Duplicates from Sorted Array80Remove Duplicates from Sorted Array II277Find the Celebrity189Rotate Array41First Missing Positive299Bulls and Cows134Gas Station118Pascal’s Triangle很少考119Pascal’s Triangle II很少考169Majority Element很少考229Majority Element II很少考274H-Index275H-Index IIBinary Search243Shortest Word Distance244Shortest Word Distance II245Shortest Word Distance III217Contains Duplicate219Contains Duplicate II很少考220Contains Duplicate III很少考55Jump Game45Jump Game II121Best Time to Buy and Sell Stock122Best Time to Buy and Sell Stock II123Best Time to Buy and Sell Stock III188Best Time to Buy and Sell Stock IV309Best Time to Buy and Sell Stock with Cooldown11Container With Most Water42Trapping Rain Water334Increasing Triplet Subsequence128Longest Consecutive Sequence164Maximum GapBucket287Find the Duplicate Number135Candy很少考330Patching Array很少考提高4Median of Two Sorted Arrays321Create Maximum Number很少考327Count of Range Sum很少考289Game of LifeInterval57Insert Interval56Merge Intervals252Meeting Rooms253Meeting Rooms II352Data Stream as Disjoint IntervalsTreeMapCounter239Sliding Window Maximum295Find Median from Data Stream53Maximum Subarray325Maximum Size Subarray Sum Equals k209Minimum Size Subarray Sum238Product of Array Except Self152Maximum Product Subarray228Summary Ranges163Missing RangesSort88Merge Sorted Array75Sort Colors283Move Zeroes376Wiggle Subsequence280Wiggle Sort324Wiggle Sort II

# 1.3.2. String

**基础**28Implement strStr()14Longest Common Prefix58Length of Last Word387First Unique Character in a String383Ransom Note344Reverse String151Reverse Words in a String186Reverse Words in a String II345Reverse Vowels of a String205Isomorphic Strings293Flip Game294Flip Game II290Word Pattern242Valid Anagram49Group Anagrams249Group Shifted Strings87Scramble String179Largest Number很少考6ZigZag Conversion很少考161One Edit Distance38Count and Say358Rearrange String k Distance Apart316Remove Duplicate Letters271Encode and Decode Strings168Excel Sheet Column Title171Excel Sheet Column Number13Roman to Integer12Integer to Roman273Integer to English Words246Strobogrammatic Number247Strobogrammatic Number II248Strobogrammatic Number III很少考提高68Text Justification65Valid Number157Read N Characters Given Read4158Read N Characters Given Read4 II — Call multiple timesSubstring76Minimum Window SubstringSliding Window30Substring with Concatenation of All WordsSliding Window3Longest Substring Without Repeating CharactersSliding Window340Longest Substring with At Most K Distinct CharactersSliding Window395Longest Substring with At Least K Repeating CharactersSliding Window159Longest Substring with At Most Two Distinct CharactersSliding WindowPalindrome125Valid Palindrome266Palindrome Permutation5Longest Palindromic Substring9Palindrome Number214Shortest Palindrome336Palindrome Pairs131Palindrome Partitioning132Palindrome Partitioning II267Palindrome Permutation IIParentheses20Valid Parentheses22Generate Parentheses32Longest Valid Parentheses241Different Ways to Add Parentheses301Remove Invalid ParenthesesSubsequence392Is Subsequence115Distinct Subsequences187Repeated DNA Sequences很少考

# 1.3.3. Math

**基础**7Reverse Integer165Compare Version Numbers66Plus One8String to Integer (atoi)258Add Digits67Add Binary43Multiply Strings29Divide Two Integers69Sqrt(x)50Pow(x, n)367Valid Perfect Square365Water and Jug Problem204Count PrimesSum1Two Sum167Two Sum II — Input array is sorted153Sum163Sum Closest很少考2593Sum Smaller很少考184Sum很少考231Power of Two326Power of Three342Power of Four372Super Pow233Number of Digit One319Bulb Switcher292Nim Game202Happy Number400Nth Digit263Ugly Number264Ugly Number II306Additive Number172Factorial Trailing Zeroes343Integer Break396Rotate Function390Elimination Game386Lexicographical Numbers357Count Numbers with Unique Digits360Sort Transformed Array397Integer Replacement368Largest Divisible Subset

# 1.3.4. Tree

**基础**144Binary Tree Preorder Traversalpreorder94Binary Tree Inorder TraversalInorder145Binary Tree Postorder Traversalpostorder102Binary Tree Level Order TraversalDFS + BFSPreorder100Same Treepreorder101Symmetric Treepreorder226Invert Binary Treepreorder + BFS257Binary Tree Pathspreorder112Path Sumpreorder113Path Sum IIpreorder129Sum Root to Leaf Numberspreorder298Binary Tree Longest Consecutive Sequencepreorder111Minimum Depth of Binary TreepreorderPostorder104Maximum Depth of Binary Treepostorder110Balanced Binary Treepostorder124Binary Tree Maximum Path Sumpostorder250Count Univalue Subtreespostorder366Find Leaves of Binary Treepostorder337House Robber IIIpostorder + preorderBFS107Binary Tree Level Order Traversal IIBFS103Binary Tree Zigzag Level Order TraversalBFS199Binary Tree Right Side ViewBFS + preorderBST98Validate Binary Search Treepreorder235Lowest Common Ancestor of a Binary Search Treepreorder236Lowest Common Ancestor of a Binary Treepostorder108Convert Sorted Array to Binary Search Treebinary search109Convert Sorted List to Binary Search Treebinary search173Binary Search Tree Iteratorinorder230Kth Smallest Element in a BSTinorder297Serialize and Deserialize Binary TreeBFS285Inorder Successor in BSTinorder270Closest Binary Search Tree Valuepreorder272Closest Binary Search Tree Value IIinorder99Recover Binary Search Treeinorder重要程度156Binary Tree Upside Down很少考114Flatten Binary Tree to Linked List很少考255Verify Preorder Sequence in Binary Search Tree很少考333Largest BST Subtree很少考222Count Complete Tree Nodes很少考105Construct Binary Tree from Preorder and Inorder Traversal很少考106Construct Binary Tree from Inorder and Postorder Traversal很少考116Populating Next Right Pointers in Each Node重要117Populating Next Right Pointers in Each Node II重要314Binary Tree Vertical Order Traversal重要96Unique Binary Search Trees重要95Unique Binary Search Trees II很少考331Verify Preorder Serialization of a Binary Tree很少考

# 1.3.5. Backtracking

**基础**78Subsets90Subsets II77Combinations39Combination Sum40Combination Sum II216Combination Sum III377Combination Sum IVDynamic Programming254Factor Combinations46Permutations47Permutations II31Next PermutationString60Permutation SequenceString291Word Pattern IIEnumeration17Letter Combinations of a Phone Number320Generalized Abbreviation要重录93Restore IP Addresses很少考282Expression Add Operators140Word Break II351Android Unlock Patterns

# 1.3.6. Dynamic Programming

**一维**70Climbing Stairs62Unique Paths63Unique Paths II120Triangle很少考279Perfect Squares139Word Break375Guess Number Higher or Lower II312Burst Balloons322Coin Change二维256Paint House265Paint House II64Minimum Path Sum72Edit Distance97Interleaving String174Dungeon Game221Maximal Square85Maximal Rectangle363Max Sum of Rectangle No Larger Than KTreeSet化简198House Robber213House Robber II276Paint Fence91Decode Ways10Regular Expression Matching44Wildcard Matching

# 1.3.7. LinkedList

**基础**206Reverse Linked List141Linked List Cycle24Swap Nodes in Pairs328Odd Even Linked List92Reverse Linked List II237Delete Node in a Linked List19Remove Nth Node From End of List83Remove Duplicates from Sorted List203Remove Linked List Elements82Remove Duplicates from Sorted List II369Plus One Linked List2Add Two Numbers160Intersection of Two Linked Lists21Merge Two Sorted Lists提高234Palindrome Linked List143Reorder List142Linked List Cycle II148Sort List25Reverse Nodes in k-Group61Rotate List86Partition List23Merge k Sorted Lists147Insertion Sort List

# 1.3.8. Binary Search

**基础**278First Bad Version35Search Insert Position33Search in Rotated Sorted Array81Search in Rotated Sorted Array II153Find Minimum in Rotated Sorted Array154Find Minimum in Rotated Sorted Array II162Find Peak Element374Guess Number Higher or Lower34Search for a Range349Intersection of Two Arrays350Intersection of Two Arrays II315Count of Smaller Numbers After Self300Longest Increasing Subsequence354Russian Doll Envelopes

# 1.3.9. Matrix

**48Rotate Image**54Spiral Matrix59Spiral Matrix II73Set Matrix Zeroes311Sparse Matrix Multiplication329Longest Increasing Path in a Matrix378Kth Smallest Element in a Sorted Matrix74Search a 2D Matrix240Search a 2D Matrix II370Range Addition79Word Search296Best Meeting Point361Bomb Enemy317Shortest Distance from All Buildings302Smallest Rectangle Enclosing Black Pixels36Valid Sudoku37Sudoku Solver

# 1.3.10. DFS & BFS

**基础**200Number of Islands286Walls and Gates130Surrounded Regions339Nested List Weight Sum364Nested List Weight Sum II127Word Ladder51N-Queens52N-Queens II126Word Ladder II

# 1.3.11. Stack & PriorityQueue

**Stack**155Min Stack232Implement Queue using Stacks225Implement Stack using Queues150Evaluate Reverse Polish Notation71Simplify Path388Longest Absolute File Path394Decode String224Basic Calculator227Basic Calculator II385Mini Parser84Largest Rectangle in HistogramPriorityQueue215Kth Largest Element in an Array347Top K Frequent Elements313Super Ugly Number很少考373Find K Pairs with Smallest Sums很少考218The Skyline Problem332Reconstruct Itinerary341Flatten Nested List Iterator

# 1.3.12. Bit Manipulation

**基础**389Find the Difference136Single Number318Maximum Product of Word Lengths很少考393UTF-8 Validation201Bitwise AND of Numbers Range371Sum of Two Integers338Counting Bits89Gray Code268Missing Number191Number of 1 Bits190Reverse Bits137Single Number II260Single Number III

# 1.3.13. Topological Sort

**基础**207Course Schedule210Course Schedule II269Alien Dictionary

# 1.3.14. Random

**基础**模板Reservoir Sampling384Shuffle an Array398Random Pick Index382Linked List Random Node380Insert Delete GetRandom O(1)381Insert Delete GetRandom O(1) — Duplicates allowed138Copy List with Random Pointer

# 1.3.15. Graph

**基础**133Clone Graph399Evaluate Division310Minimum Height Trees图形学335Self Crossing很少考149Max Points on a Line356Line Reflection很少考391Perfect Rectangle很少考223Rectangle Area很少考

# 1.3.16. Union FInd

**基础**261Graph Valid Tree323Number of Connected Components in an Undirected Graph305Number of Islands II

# 1.3.17. Trie

**基础**211Add and Search Word — Data structure design208Implement Trie (Prefix Tree)212Word Search II

# 1.3.18. Design

**基础**359Logger Rate Limiter346Moving Average from Data StreamSliding Window362Design Hit Counter281Zigzag Iterator284Peeking Iterator251Flatten 2D Vector288Unique Word Abbreviation170Two Sum III — Data structure design348Design Tic-Tac-Toe379Design Phone Directory353Design Snake Game146LRU Cache355Design Twitter303Range Sum Query — Immutable304Range Sum Query 2D — Immutable307Range Sum Query — MutableBinary Index Tree308Range Sum Query 2D — MutableBinary Index Tree

# 1.4. Leetcode 前 400 重点 250 题

# 1.4.1. 划分说明

这个重点题目是我个人把Leetcode前400题进行精简，精简方法如下：

* 删除不常考，面试低频出现题目
* 删除重复代码题目（例：链表反转206题，代码在234题出现过）
* 删除过于简单题目（例：100题：Same Tree）
* 删除题意不同，代码基本相同题目（例：136 & 389，保留一个）

所有题目我尽量保证客观公正，只是按大概率删除不常考题目，很多题目面经出现过，但出现次数属于个位数或者只有一两家出现。所以**如在面试中出现删除题目概不负责**，这只是从**概率**上删除低频，简单题目。旨在减轻大家的刷题负担，从400题减少到250题。

适用人群：有一定刷题基础，算法基础，**二刷人群**。

**建议：400题全部刷完，再精刷这250题。**

# 1.4.2. 重点题目

**IDTitle**1Two Sum3Longest Substring Without Repeating Characters4Median of Two Sorted Arrays5Longest Palindromic Substring7Reverse Integer8String to Integer (atoi)10Regular Expression Matching11Container With Most Water12Integer to Roman13Roman to Integer153Sum17Letter Combinations of a Phone Number184Sum20Valid Parentheses22Generate Parentheses23Merge k Sorted Lists26Remove Duplicates from Sorted Array27Remove Element28Implement strStr()29Divide Two Integers31Next Permutation32Longest Valid Parentheses33Search in Rotated Sorted Array34Search for a Range35Search Insert Position36Valid Sudoku37Sudoku Solver38Count and Say39Combination Sum40Combination Sum II41First Missing Positive42Trapping Rain Water43Multiply Strings44Wildcard Matching45Jump Game II46Permutations47Permutations II48Rotate Image49Group Anagrams50Pow(x, n)51N-Queens52N-Queens II53Maximum Subarray54Spiral Matrix55Jump Game56Merge Intervals57Insert Interval59Spiral Matrix II60Permutation Sequence62Unique Paths64Minimum Path Sum65Valid Number66Plus One67Add Binary68Text Justification69Sqrt(x)70Climbing Stairs71Simplify Path72Edit Distance74Search a 2D Matrix75Sort Colors76Minimum Window Substring77Combinations78Subsets79Word Search80Remove Duplicates from Sorted Array II81Search in Rotated Sorted Array II82Remove Duplicates from Sorted List II84Largest Rectangle in Histogram85Maximal Rectangle88Merge Sorted Array90Subsets II91Decode Ways96Unique Binary Search Trees98Validate Binary Search Tree101Symmetric Tree104Maximum Depth of Binary Tree108Convert Sorted Array to Binary Search Tree110Balanced Binary Tree111Minimum Depth of Binary Tree112Path Sum113Path Sum II115Distinct Subsequences116Populating Next Right Pointers in Each Node117Populating Next Right Pointers in Each Node II121Best Time to Buy and Sell Stock122Best Time to Buy and Sell Stock II123Best Time to Buy and Sell Stock III124Binary Tree Maximum Path Sum125Valid Palindrome126Word Ladder II127Word Ladder128Longest Consecutive Sequence130Surrounded Regions133Clone Graph134Gas Station138Copy List with Random Pointer139Word Break140Word Break II142Linked List Cycle II146LRU Cache149Max Points on a Line150Evaluate Reverse Polish Notation152Maximum Product Subarray153Find Minimum in Rotated Sorted Array154Find Minimum in Rotated Sorted Array II155Min Stack157Read N Characters Given Read4158Read N Characters Given Read4 II — Call multiple times161One Edit Distance162Find Peak Element163Missing Ranges168Excel Sheet Column Title171Excel Sheet Column Number173Binary Search Tree Iterator186Reverse Words in a String II174Dungeon Game188Best Time to Buy and Sell Stock IV189Rotate Array191Number of 1 Bits198House Robber200Number of Islands201Bitwise AND of Numbers Range202Happy Number204Count Primes205Isomorphic Strings207Course Schedule208Implement Trie (Prefix Tree)209Minimum Size Subarray Sum210Course Schedule II211Add and Search Word — Data structure design212Word Search II213House Robber II214Shortest Palindrome215Kth Largest Element in an Array216Combination Sum III217Contains Duplicate218The Skyline Problem219Contains Duplicate II220Contains Duplicate III221Maximal Square224Basic Calculator225Implement Stack using Queues226Invert Binary Tree227Basic Calculator II228Summary Ranges230Kth Smallest Element in a BST231Power of Two232Implement Queue using Stacks235Lowest Common Ancestor of a Binary Search Tree236Lowest Common Ancestor of a Binary Tree238Product of Array Except Self239Sliding Window Maximum240Search a 2D Matrix II241Different Ways to Add Parentheses242Valid Anagram244Shortest Word Distance II245Shortest Word Distance III249Group Shifted Strings251Flatten 2D Vector252Meeting Rooms253Meeting Rooms II254Factor Combinations256Paint House257Binary Tree Paths261Graph Valid Tree263Ugly Number264Ugly Number II265Paint House II268Missing Number269Alien Dictionary270Closest Binary Search Tree Value271Encode and Decode Strings273Integer to English Words274H-Index275H-Index II276Paint Fence277Find the Celebrity278First Bad Version279Perfect Squares280Wiggle Sort282Expression Add Operators283Move Zeroes284Peeking Iterator285Inorder Successor in BST286Walls and Gates287Find the Duplicate Number288Unique Word Abbreviation289Game of Life290Word Pattern291Word Pattern II293Flip Game294Flip Game II295Find Median from Data Stream296Best Meeting Point297Serialize and Deserialize Binary Tree298Binary Tree Longest Consecutive Sequence299Bulls and Cows300Longest Increasing Subsequence301Remove Invalid Parentheses302Smallest Rectangle Enclosing Black Pixels305Number of Islands II307Range Sum Query — Mutable308Range Sum Query 2D — Mutable309Best Time to Buy and Sell Stock with Cooldown311Sparse Matrix Multiplication312Burst Balloons314Binary Tree Vertical Order Traversal316Remove Duplicate Letters317Shortest Distance from All Buildings318Maximum Product of Word Lengths322Coin Change323Number of Connected Components in an Undirected Graph324Wiggle Sort II325Maximum Size Subarray Sum Equals k329Longest Increasing Path in a Matrix334Increasing Triplet Subsequence336Palindrome Pairs337House Robber III338Counting Bits339Nested List Weight Sum340Longest Substring with At Most K Distinct Characters341Flatten Nested List Iterator346Moving Average from Data Stream347Top K Frequent Elements348Design Tic-Tac-Toe350Intersection of Two Arrays II351Android Unlock Patterns352Data Stream as Disjoint Intervals353Design Snake Game354Russian Doll Envelopes355Design Twitter359Logger Rate Limiter361Bomb Enemy362Design Hit Counter364Nested List Weight Sum II367Valid Perfect Square374Guess Number Higher or Lower375Guess Number Higher or Lower II376Wiggle Subsequence377Combination Sum IV378Kth Smallest Element in a Sorted Matrix379Design Phone Directory380Insert Delete GetRandom O(1)381Insert Delete GetRandom O(1) — Duplicates allowed384Shuffle an Array385Mini Parser389Find the Difference394Decode String398Random Pick Index

Download Link: https://assignmentchef.com/product/solved-cse302-project-02-sorting-list-based-strings-and-numbers
<br>
<strong>Overview</strong>

<strong>Your second project will be to build a sorting utility called </strong><strong>volsort </strong><strong>in C++ that will mimic the UNIX tool “sort” with a twist: the underlying container must </strong><strong>be a linked list (</strong><a href="https://en.wikipedia.org/wiki/Linked_list)."><strong>https://en.wikipedia.org/wiki/Linked_list).</strong></a>

<strong>Your program must implement the following sorting methods or modes:</strong>

<ol>

 <li><strong>Oblivious: This sorting method does no sorting; it just reads and returns the numbers similar to the benchmark in Dr. Plank’s notes.</strong></li>

 <li><strong>STL: This sorting method simply uses the std::sort (</strong><a href="http://en.cppreference.com/w/cpp/algorithm/sort)"><strong>http://en.cppreference.com/w/cpp/algorithm/sort)</strong></a></li>

 <li><strong>qsort: This sorting method simply uses the qsort (</strong><a href="http://man7.org/linux/man-pages/man3/qsort.3.html)"><strong>http://man7.org/linux/man-pages/man3/qsort.3.html)</strong></a></li>

 <li><strong>merge: This sorting method uses a custom implementation of the merge sort (</strong><a href="https://en.wikipedia.org/wiki/Merge_sort)"><strong>https://en.wikipedia.org/wiki/Merge_sort)</strong></a></li>

 <li><strong>quick: This sorting method uses a custom implementation of the quick sort (</strong><a href="https://en.wikipedia.org/wiki/Quicksort)"><strong>https://en.wikipedia.org/wiki/Quicksort)</strong></a> <strong>This project is to be done in groups of 1 – 4 students and is due Wednesday February 6th, 2019 at 12:01pm.</strong></li>

</ol>

<strong>Specification</strong>

<strong>My implementation, which you should mimic, has the following usage message:</strong>

<strong>$ volsort -h </strong><strong>usage: volsort</strong>

<ul>

 <li><strong>m MODE </strong><strong>Sorting mode (oblivious, stl, qsort, merge, quick)</strong></li>

 <li><strong>n Perform numerical ordering</strong></li>

</ul>

<strong>volsort </strong><strong>therefore must take two optional arguments: the -m flag, which lets the user select which sorting method to use; and the </strong><strong>–</strong><strong>n </strong><strong>flag, which informs the program to treat the input values as numeric integers rather than textual strings.</strong>

<strong>As with most Unix filters (</strong><a href="https://en.wikipedia.org/wiki/Filter_(software)#Unix),"><strong>https://en.wikipedia.org/wiki/Filter_(software)#Unix),</strong></a> <strong>volsort </strong><strong>reads from [standand input] and then outputs to standard output </strong><strong>(</strong><a href="https://en.wikipedia.org/wiki/Standard_streams#Standard_output_.28stdout.29):"><strong>https://en.wikipedia.org/wiki/Standard_streams#Standard_output_.28stdout.29):</strong></a>




<strong># Use the volsort solution to order 10 random numbers in the file input</strong>

<strong>$ volsort -m quick &lt; input</strong>

<strong>12 </strong><strong>32 37 37 50 </strong><strong>61 69 71 90 97</strong>

<strong>Node and List</strong>

<strong>Your header file should contains the definition of the </strong><strong>struct </strong><strong>Node </strong><strong>and </strong><strong>struct </strong><strong>List </strong><strong>such as this:</strong>

<strong>struct Node {</strong>

<strong>std::string string;</strong><strong>int         </strong><strong>number;</strong>

<strong>Node      </strong><strong>*next;</strong>

<strong>};</strong>

<strong>struct List {</strong>

<strong>Node      </strong><strong>*head;</strong>

<strong>size_t     </strong><strong>size;</strong>

<strong>List(); </strong><strong>—List();</strong>

<strong>void push_front(const std::string &amp;s);</strong>

<strong>};</strong>

<strong>This </strong><strong>struct List </strong><strong>is a singly-linked list (</strong><a href="https://en.wikipedia.org/wiki/Linked_list#Singly_linked_lists)"><strong>https://en.wikipedia.org/wiki/Linked_list#Singly_linked_lists)</strong></a><strong> with a constructor, destructor, and a single </strong><strong>push_front method. This </strong><strong>struct </strong><strong>List </strong><strong>consists of </strong><strong>struct </strong><strong>Node </strong><strong>entries that contain both string values, the corresponding number value, and a pointer </strong><strong>to the next </strong><strong>struct Node .</strong>

<strong>It is required that you implement the </strong><strong>struct List </strong><strong>constructor, destructor, and push_front method. The push_front method should insert a new </strong><strong>struct Node </strong><strong>into the front of the list. To get the number value for the new </strong><strong>struct Node , </strong><strong>you should use the std::stoi (</strong><a href="http://en.cppreference.com/w/cpp/string/basic_string/stol)"><strong>http://en.cppreference.com/w/cpp/string/basic_string/stol)</strong></a><strong> function (if this conversion fails, you can default to 0 for the number value).</strong>

<strong>You should implement the </strong><strong>struct Node </strong><strong>comparison functions in C ++ style as:</strong>

<strong><em>// C++ StyLe comparison function</em></strong>

<strong>bool node_number_compare(const Node *a, const Node *b);bool node_string_compare(const Node *a, const Node *b);</strong>

<strong>Thus, there are two comparision functions in total: one for comparing the number field and the other for comparing the string field of the </strong><strong>struct Node </strong><strong>Additionally, it will help to debug if you also implement the </strong><strong>dump_node </strong><strong>utility function which outputs the contents of each node start from </strong><strong>n </strong><strong>until </strong><strong>nullptr </strong><strong>is reached:</strong>

<strong><em>// </em></strong><strong><em>UtiLity function</em></strong>

<strong>void dump_node(Node *n);</strong>

<strong>STL Sort</strong>

<strong>The first sorting mode is to use the std::sort (</strong><a href="http://en.cppreference.com/w/cpp/algorithm/sort)"><strong>http://en.cppreference.com/w/cpp/algorithm/sort)</strong></a><strong> method. Because our </strong><strong>struct </strong><strong>List </strong><strong>does not implement </strong><strong>random access iterators, we will need to copy the </strong><strong>struct Node </strong><strong>s in the </strong><strong>struct </strong><strong>List </strong><strong>into another container such as an array. Once we have this copy, </strong><strong>we can then call the std::sort (</strong><a href="http://en.cppreference.com/w/cpp/algorithm/sort)"><strong>http://en.cppreference.com/w/cpp/algorithm/sort)</strong></a><strong> function on this copy with the appropriate comparison function. Note that </strong><strong>our Node struct has three values, one of which is a string. The most efficient way to reconstitute the sorted order is to update the links between the </strong><strong>struct Node </strong><strong>s — not to simply replacing the list data with node data. For example, for a list of <em>n </em>strings you may have to run atoi <em>n </em>times, versus simply </strong><strong>updating <em>n </em>pointers as suggested here.</strong>

<strong>Hint: You should store </strong><strong>struct Node </strong><strong>* in your copy array.</strong>

<strong>Hint: Make sure you set the head of the </strong><strong>struct List </strong><strong>after you have set the links of all the </strong><strong>struct Node </strong><strong>s.</strong>

<strong>QSort</strong>

<strong>The second sorting mode could be nearly identical to the first function, except you will use qsort (</strong><a href="http://man7.org/linux/man-pages/man3/qsort.3.html)"><strong>http://man7.org/linux/man-pages/man3/qsort.3.html)</strong></a> <strong>instead of std::sort (</strong><a href="http://en.cppreference.com/w/cpp/algorithm/sort)."><strong>http://en.cppreference.com/w/cpp/algorithm/sort).</strong></a>

<strong>Merge Sort</strong>

<strong>Both of the custom merge sort (</strong><a href="https://en.wikipedia.org/wiki/Merge_sort)"><strong>https://en.wikipedia.org/wiki/Merge_sort)</strong></a><strong> and quick sort (</strong><a href="https://en.wikipedia.org/wiki/Quicksort)"><strong>https://en.wikipedia.org/wiki/Quicksort)</strong></a><strong> implementations must use the following divide-and-conquer (</strong><a href="https://en.wikipedia.org/wiki/Divide_and_conquer_algorithms)"><strong>https://en.wikipedia.org/wiki/Divide_and_conquer_algorithms)</strong></a><strong> strategy. Msort is used here as the initial example:</strong>




<table>

 <tbody>

  <tr>

   <td width="796">

    <table width="100%">

     <tbody>

      <tr>

       <td></td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>

<strong>Node *msort(Node *head, CompareFunction compare) { </strong><strong><em>// HandLe base case</em></strong>

<strong><em>// Divide into Left and right subLists </em></strong><strong><em>// Conquer Left and right subLists</em></strong>

<strong><em>// Combine Left and right subLists</em></strong>

}

<strong>Specifically, you could implement the third sorting mode as follows:</strong>

<strong><em>// PubLic functions</em></strong>

<strong>void merge_sort(List &amp;l, bool numeric);</strong>

<strong><em>// InternaL functions</em></strong>

<strong>Node *msort(Node *head, CompareFunction compare);</strong>

<strong>void split(Node *head, Node *&amp;left, Node *&amp;right);</strong>

<strong>Node *merge(Node *left, Node *right, CompareFunction compare);</strong>

<ul>

 <li><strong>merge_sort </strong><strong>takes a </strong><strong>struct List </strong><strong>and whether or not the comparison should be </strong><strong>numeric </strong><strong>and performs the top-down merge sort </strong><strong>(</strong><a href="https://en.wikipedia.org/wiki/Merge_sort)"><strong>https://en.wikipedia.org/wiki/Merge_sort)</strong></a><strong> This function serves as a wrapper or helper function for the recursive </strong><strong>msort </strong><strong>function.</strong></li>

 <li><strong>msort </strong><strong>is the recursive portion of the algorithm and calls </strong><strong>split </strong><strong>to </strong><strong>divide </strong><strong>and calls </strong><strong>merge </strong><strong>to </strong> <strong>It returns the new </strong><strong>head </strong><strong>of the list.</strong></li>

 <li><strong>split </strong><strong>is a helper function that splits the singly-linked list (</strong><a href="https://en.wikipedia.org/wiki/Linked_list#Singly_linked_lists)"><strong>https://en.wikipedia.org/wiki/Linked_list#Singly_linked_lists)</strong></a><strong> in half by using the <em>slow </em>and </strong><strong><em>faster </em></strong><strong>pointer trick (</strong><a href="https://www.quora.com/What-is-a-slow-pointer-and-a-fast-pointer-in-a-linked-list)"><strong>https://www.quora.com/What-is-a-slow-pointer-and-a-fast-pointer-in-a-linked-list)</strong></a><strong> (aka [tortoise and hare]).</strong></li>

 <li><strong>merge </strong><strong>is a helper function that combines both the </strong><strong>left </strong><strong>and </strong><strong>right </strong><strong>lists and returns the new </strong><strong>head </strong><strong>of the list.</strong></li>

</ul>

<strong>Online Code — don’t look!</strong>

<strong>There are many examples of merge sort (</strong><a href="https://en.wikipedia.org/wiki/Merge_sort)"><strong>https://en.wikipedia.org/wiki/Merge_sort)</strong></a><strong> on linked lists. For instance, GeeksForGeeks (</strong><a href="http://www.geeksforgeeks.org/merge-sort-for-linked-list/)"><strong>http://www.geeksforgeeks.org/merge-sort-for-linked-list/)</strong></a><strong> has an article about Merge Sort for Linked Lists (</strong><a href="http://www.geeksforgeeks.org/merge-sort-for-linked-list/)"><strong>http://www.geeksforgeeks.org/merge­</strong></a><strong><u>sort-for-linked-list/)</u></strong><strong>. You should resist the temptation to look at such sources.</strong>

<strong>Remember that any code you submit must be your own and that you should understand everything you submit. The best way to do that is to not </strong><strong>look at online solutions.</strong>

<strong>If you still don’t believe me, the case above from GeeksForGeeks (</strong><a href="http://www.geeksforgeeks.org/merge-sort-for-linked-list/)"><strong>http://www.geeksforgeeks.org/merge-sort-for-linked-list/)</strong></a><strong> is suboptimal for two reasons: 1) We are not covering or requiring recursion, so its a red flag; and 2) the reason we are stressing an iterative version of </strong><strong>merge </strong><strong>for this project is to ensure you don’t overflow the stack on large input sizes we will provide during grading. We recognize libraries can be useful but we are</strong>




<strong>7/12/2020                                                                                                                                                                                                                                                                                      </strong><strong>Project 02: Sorting List-Based Strings and Numbers</strong>

<strong>requiring you to implement this on your own.</strong>

<strong>Quick Sort</strong>

<strong>The fourth sorting mode is a custom implementation of quick sort (</strong><a href="https://en.wikipedia.org/wiki/Quicksort)"><strong>https://en.wikipedia.org/wiki/Quicksort)</strong></a><strong>. You are to implement a simple version of the </strong><strong>algorithm where the first element is the pivot.</strong>

<strong><em>// PubLic functions</em></strong>

<strong>void quick_sort(List &amp;l, bool numeric);</strong>

<strong><em>// InternaL functions</em></strong>

<strong>Node *qsort(Node *head, CompareFunction compare)</strong>

<strong>void partition(Node *head, Node *pivot, Node *&amp;left, Node *&amp;right, CompareFunction compare); </strong><strong>Node *concatenate(Node *left, Node *right);</strong>

<ul>

 <li><strong>quick_sort </strong><strong>takes a </strong><strong>struct List </strong><strong>and whether or not the comparison should be </strong><strong>numeric </strong><strong>and performs the quick sort </strong><strong>(</strong><a href="https://en.wikipedia.org/wiki/Quicksort)"><strong>https://en.wikipedia.org/wiki/Quicksort)</strong></a><strong> This function serves as a wrapper or helper function for the recursive </strong><strong>qsort </strong><strong>function.</strong></li>

 <li><strong>qsort </strong><strong>is the recursive portion of the algorithm and calls partition to divide the list and calls </strong><strong>concatenate </strong><strong>to conquer. It returns the new </strong><strong>head </strong><strong>of the list.</strong></li>

 <li><strong>partition </strong><strong>is a helper function that splits the singly-linked list (</strong><a href="https://en.wikipedia.org/wiki/Linked_list#Singly_linked_lists)"><strong>https://en.wikipedia.org/wiki/Linked_list#Singly_linked_lists)</strong></a><strong> into two </strong><strong>left </strong><strong>and </strong><strong>right </strong><strong>lists such that all elements less than the </strong><strong>pivot </strong><strong>are in the </strong><strong>left </strong><strong>side and everything else is on the </strong><strong>right .</strong></li>

 <li><strong>concatenate </strong><strong>is a helper function that combines both the </strong><strong>left </strong><strong>and </strong><strong>right </strong><strong>lists and returns the new </strong><strong>head </strong><strong>of the list.</strong></li>

</ul>

<strong>Online Code — don’t look!</strong>

<strong>Again, there are many examples of quick sort (</strong><a href="https://en.wikipedia.org/wiki/Quicksort)"><strong>https://en.wikipedia.org/wiki/Quicksort)</strong></a><strong> on linked lists. For instance, GeeksForGeeks </strong><strong>(</strong><a href="http://www.geeksforgeeks.org/merge-sort-for-linked-list/)"><strong>http://www.geeksforgeeks.org/merge-sort-for-linked-list/)</strong></a><strong> has an article about QuickSort on Singly Linked Lists (</strong><a href="http://www.geeksforgeeks.org/quicksort-on-singly-linked-list/)"><strong>http://www.geeksforgeeks.org/quicksort-on-singly-linked-list/)</strong></a><strong>. Again, you should resist the temptation to look at such sources.</strong>

<strong>Just like above, this example from GeeksForGeeks (</strong><a href="http://www.geeksforgeeks.org/merge-sort-for-linked-list/)"><strong>http://www.geeksforgeeks.org/merge-sort-for-linked-list/)</strong></a><strong> is much more complex than the </strong><strong>proposed divide-and-conquer (</strong><a href="https://en.wikipedia.org/wiki/Divide_and_conquer_algorithms)"><strong>https://en.wikipedia.org/wiki/Divide_and_conquer_algorithms)</strong></a><strong> strategy shown above.</strong>

<strong>Finishing up</strong>

<strong>To test your programs, simply run </strong><strong>make test </strong><strong>. Your programs must correctly process the </strong><strong>input </strong><strong>correctly, have no memory errors, and pass the test </strong><strong>cases provided.</strong>




<strong>7/12/2020                                                                                                                                                                                                                                                                                      </strong><strong>Project 02: Sorting List-Based Strings and Numbers</strong>

<strong>When you are finished implementing your </strong><strong>volsort </strong><strong>sorting utility answer the following questions in the </strong><strong>project02/README.md :</strong>

<ol>

 <li><strong> Benchmark all four different sorting modes on files with the following number of integers:</strong></li>

</ol>

10, 100, 1000, 10000, 100000, 1000000, 10000000

<strong>Record the elapsed time and memory consumption of each mode and file size in a Markdown (</strong><a href="https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)"><strong>https://github.com/adam-p/markdown­</strong></a><strong><u>here/wiki/Markdown-Cheatsheet)</u></strong><strong> table:</strong>




<table>

 <tbody>

  <tr>

   <td width="144"><strong>Mode</strong></td>

   <td width="57"><strong>Size</strong></td>

   <td colspan="2" width="691"><strong>Elapsed Time</strong></td>

  </tr>

  <tr>

   <td width="144"><strong>STL</strong></td>

   <td width="57">10</td>

   <td width="25">0</td>

   <td width="666"></td>

  </tr>

  <tr>

   <td width="144"><strong>STL</strong></td>

   <td width="57">100</td>

   <td width="25">0</td>

   <td width="666"></td>

  </tr>

  <tr>

   <td width="144"><strong>…</strong></td>

   <td width="57">·    • •</td>

   <td width="25">·    •</td>

   <td width="666">•</td>

  </tr>

  <tr>

   <td width="144"><strong>QSORT</strong></td>

   <td width="57">10</td>

   <td width="25">0</td>

   <td width="666"></td>

  </tr>

  <tr>

   <td width="144"><strong>…</strong></td>

   <td width="57">·    • •</td>

   <td width="25">·    •</td>

   <td width="666">•</td>

  </tr>

  <tr>

   <td width="144"><strong>MERGE</strong></td>

   <td width="57">10</td>

   <td width="25">0</td>

   <td width="666"></td>

  </tr>

  <tr>

   <td width="144"><strong>…</strong></td>

   <td width="57">·    • •</td>

   <td width="25">·    •</td>

   <td width="666">•</td>

  </tr>

  <tr>

   <td width="144"><strong>QUICK</strong></td>

   <td width="57">10</td>

   <td width="25">0</td>

   <td width="666"></td>

  </tr>

 </tbody>

</table>




<table>

 <tbody>

  <tr>

   <td width="129"><strong>…</strong></td>

   <td width="64">·    • •</td>

   <td width="698">·  • •</td>

  </tr>

 </tbody>

</table>




<strong>Hint: Use the Unix utility time to get the elapsed time per test.</strong>

<strong>Hint: Write a simple C/C++ program or create a Python (</strong><a href="https://www.python.org/)"><strong>https://www.python.org/)</strong></a><strong> script to generate the input files. </strong><strong>Optional: If you have prior experience, write another simple script (shell script for example) to automate the benchmarking.</strong>

<strong>Scratch Space and Output</strong>

<strong>You may wish to create your test files in </strong><strong>/tmp </strong><strong>as that is likely larger than your EECS local directory. </strong><strong>Likewise, you should redirect the output of </strong><strong>volsort </strong><strong>to </strong><strong>/dev/null </strong><strong>for the benchmarking.</strong>

<ol start="2">

 <li><strong> After you have performed your benchmark:</strong></li>

</ol>

<ul>

 <li><strong>Discuss the relative performance of each sorting method and try to explain the differences.</strong></li>

 <li><strong>What do these results reveal about the relationship between theoretical complexity discussed in class and actual performance?</strong></li>

 <li><strong>In your opinion, which sorting mode is the best? Justify your conclusion by examining the trade-offs for the chosen mode. </strong><strong>In addition to the questions, please provide a brief summary of each individual group members contributions to the project.</strong></li>

</ul>




<strong>Testing your code prior to submission</strong>

<strong>To faciliate testing, you were previously asked to clone the course Github repository as follows:</strong>

<strong>git clone </strong><a href="https://github.com/semrich/ds302_20.git"><strong>https://github.com/semrich/ds302_20.git</strong></a><strong> cs302</strong>

<strong>For this assignment, update this clone by using the following:</strong>

<strong>git pull</strong>

<strong>We’ll discuss this in class but note that your program must be compilable using make. To test your solution against ours, type: </strong><strong>make test</strong>

<strong>Rubric</strong>

<strong>As we grade the assignment, “as intended” means you achieve the correct output in a given mode as outlined in this project document above. We may </strong><strong>choose to assign up to 50% of the total points for as many alternative implementations, e.g., volsort -m stl copies list into a vector, sorts using std:sort(v.begin(), v.end()), and copies back into the list if:</strong>

<ol>

 <li><strong>Corresponding markdown entries are included (see part 3)</strong></li>

 <li><strong>You articulate in your writeups what challenges you had implementing the code as outlined in the project description above.</strong></li>

</ol>

<strong>Full rubric in three total parts follows: Part 1: Project high-level requirements</strong>

<ul>

 <li><strong>+2 code well formatted, commented, with reasonable variable names</strong></li>

 <li><strong>+2 You partner(s) names (if done in a group and Git repo link (1 point each)</strong></li>

 <li><strong>+4 Your writeup about the group dynamics, contributions, or summary of solution (solo projects)</strong></li>

</ul>

<strong>Part 2: Make test</strong>

<ul>

 <li><strong>+ 4 Volsort -m stl passes (no diff, code as intended)</strong></li>

 <li><strong>+ 4 Volsort -m stl -n passes (no diff, code as intended)</strong></li>

 <li><strong>+4 Volsort -m qsort passes (no diff, code as intended)</strong></li>

</ul>




<strong>7/12/2020                                                                                                                                                                                                                                                                                      </strong><strong>Project 02: Sorting List-Based Strings and Numbers</strong>

<ul>

 <li>+4 Volsort -m qsort -n passes (no diff, code as intended)</li>

 <li>+5 Volsort -m merge passes (no diff, code as intended)</li>

 <li>+5 Volsort -m merge -n passes (no diff, code as intended)</li>

 <li>+5 Volsort -m quick passes (no diff, code as intended)</li>

 <li>+5 Volsort -m quick -n passes (no diff, code as intended)</li>

 <li>+4 No reported memory issues</li>

</ul>

<strong>Part 3: </strong>Report in Git readme (as requested) or separate doc

<ul>

 <li>+6 markdown table as requested (1.5 point per sort)</li>

 <li>+6 answers to requested questions (2 points each)</li>

</ul>

<strong>Submission</strong>

To submit your solution, you must submit a single archive (.zip, .tar, .gz, etc.) on Canvas prior to the deadline. <strong>Note: Although submission will be faciliated by Canvas, we will compile and test on EECS lab machines! </strong><strong>If </strong>you develop your solution elsewhere please make sure it works on the lab computers prior to the deadline
package main

import ("golang.org/x/tour/tree"
        "fmt")

// Walk walks the tree t sending all values
// from the tree to the channel ch.
func Walk(t *tree.Tree, ch chan int, depth int) {
	if (t.Left != nil) {
		Walk(t.Left, ch, depth+1)
	}
	
	ch <- t.Value
	
	if (t.Right != nil) {
		Walk(t.Right, ch, depth+1)
	}
	
	if (depth == 0) {
		close(ch)
	}
}

// Same determines whether the trees
// t1 and t2 contain the same values.
func Same(t1, t2 *tree.Tree) bool {
	ch1 := make(chan int)
	ch2 := make(chan int)

	var n1 [10]int
	var n2 [10]int

	go Walk(t1, ch1, 0)
	
	var i = 0
	for val := range ch1 {
		n1[i] = val
		i++
	}
	
	go Walk(t2, ch2, 0)

	var j = 0
	for val := range ch2 {
		n2[j] = val
		j++
	}

	fmt.Println(n1, n2)
	if (len(n1) != len(n2)) {
		return false
	}
	
	for k:=0; k<len(n1); k++ {
		if n1[k] != n2[k] {
			return false
		}
	}

	return true
}


func main() {
	// ch := make(chan int)
	// go Walk(tree.New(1), ch, 0)

	// for i := range ch {
	//	fmt.Println(i)
	// }
	// fmt.Println(tree.New(2))
	
	var same = Same(tree.New(1), tree.New(1))
	
	fmt.Println("Same: ", same)
	
	fmt.Println("end")
}








package main

import "fmt"

type IPAddr [4]byte

// TODO: Add a "String() string" method to IPAddr.

func (ip IPAddr) String() string {
	return fmt.Sprintf("%v.%v.%v.%v", ip[0], ip[1], ip[2], ip[3])
}

func main() {
	hosts := map[string]IPAddr{
		"loopback":  {127, 0, 0, 1},
		"googleDNS": {8, 8, 8, 8},
	}
	for name, ip := range hosts {
		fmt.Printf("%v: %v\n", name, ip)
	}
}











package main

import (
	"golang.org/x/tour/wc"
	"strings"
)

func WordCount(s string) map[string]int {
	fields := strings.Fields(s)
	counts := make(map[string]int)
	for _, v := range fields {
		_, ok := counts[v]
		if ok {
			counts[v] += 1
		} else {
			counts[v] = 1
		}
	}
	return counts
}

func main() {
	wc.Test(WordCount)
}









package main

import (
	"fmt"
)

func Sqrt(x float64) float64 {
  z:=1.0
  for i:=1; i<10; i++ {
    z-= (z*z - x) / (2*z)
	fmt.Println(z)
  }
  return z
}

func main() {
	fmt.Println(Sqrt(16))
}




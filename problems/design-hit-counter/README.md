# Leetcode: Design Hit Counter     :BLOG:Medium:


---

Design Hit Counter  

---

Similar Problems:  
-   [Logger Rate Limiter](https://code.dennyzhang.com/logger-rate-limiter)
-   [Review: Object-Oriented Design Problems](https://code.dennyzhang.com/review-oodesign)
-   Tag: [oodesign](https://code.dennyzhang.com/tag/oodesign)

---

Design a hit counter which counts the number of hits received in the past 5 minutes.  

Each function accepts a timestamp parameter (in seconds granularity) and you may assume that calls are being made to the system in chronological order (ie, the timestamp is monotonically increasing). You may assume that the earliest timestamp starts at 1.  

It is possible that several hits arrive roughly at the same time.  

Example:  

    HitCounter counter = new HitCounter();
    
    // hit at timestamp 1.
    counter.hit(1);
    
    // hit at timestamp 2.
    counter.hit(2);
    
    // hit at timestamp 3.
    counter.hit(3);
    
    // get hits at timestamp 4, should return 3.
    counter.getHits(4);
    
    // hit at timestamp 300.
    counter.hit(300);
    
    // get hits at timestamp 300, should return 4.
    counter.getHits(300);
    
    // get hits at timestamp 301, should return 3.
    counter.getHits(301);

Follow up:  
What if the number of hits per second could be very large? Does your design scale?  

Github: [challenges-leetcode-interesting](https://github.com/DennyZhang/challenges-leetcode-interesting/tree/master/design-hit-counter)  

Credits To: [leetcode.com](https://leetcode.com/problems/design-hit-counter/description/)  

Leave me comments, if you have better ways to solve.  

---

    ## Blog link: https://code.dennyzhang.com/design-hit-counter
    ## Basic Ideas: hashmap
    ##
    ## Complexity: Time O(1), Space O(1)
    import collections
    class HitCounter:
    
        def __init__(self):
            """
            Initialize your data structure here.
            """
            self.d = collections.defaultdict(lambda: 0)
            self.max_key = None
    
    
        def hit(self, timestamp):
            """
            Record a hit.
            @param timestamp - The current timestamp (in seconds granularity).
            :type timestamp: int
            :rtype: void
            """
            if self.max_key is None:
                self.max_key = timestamp
    
            self.d[timestamp] += 1
    
            # cleanup very old keys
            l = []
            for key in self.d:
                if key <= self.max_key-300:
                    l.append(key)
    
            for key in l:
                del self.d[key]
    
    
        def getHits(self, timestamp):
            """
            Return the number of hits in the past 5 minutes.
            @param timestamp - The current timestamp (in seconds granularity).
            :type timestamp: int
            :rtype: int
            """
            self.max_key = timestamp
            res = 0
            # cleanup very old keys
            l = []
            for key in self.d:
                if key <= self.max_key-300:
                    l.append(key)
                else:
                    res += self.d[key]
    
            for key in l:
                del self.d[key]
    
            return res
    
    # Your HitCounter object will be instantiated and called as such:
    # obj = HitCounter()
    # obj.hit(timestamp)
    # param_2 = obj.getHits(timestamp)
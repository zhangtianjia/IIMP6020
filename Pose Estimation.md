# Pose Estimation
In this tutorial, you will learn how to estimate the pose of Tello with mission pads. 



# Marker Retrievaling and Tracking
We need to detect markers in the captured image to determine the index of the mission pad which is called retrivaling. Usually for a video,  
There are several circular dots in each mision pad with different distribution. The local arrangement of neighbor dots is unique for each dot so the arrangements can be used as descriptors of dots. W

## Keypoint Based Method
### Keypoint Matching By Locally Likely Arrangement Hashing
For each extracted point, its corresponding point is retrieved from the database. Because a dot does not have interior features to descriminate itself from other.

#### Keypoint Extraction

#### Calculation of Features

The simplest definition of the feature of a feature point $p$ is to use $f$ nearest feature points from $p$ . In general, we assume that common $m$ points exist in $n$ nearest neighbors under some extent of perspective distortion. As shown in FIG.4, common $m$ points are obtained by examining all possible combinations $P_{m(0)}$, $P_{m(1)}$,...,$P_{m(nC_{m-1})}$ of $m$ points from $n$ nearist points. As long as the assumption holds, at least one combination of $m$ points is common. Thus a stable feature can be obtained.

The simplest way of calculating the feature from $m$ points is to set $m=f$ and calculate the cross-ratio or the affine invariant from $f$ points. Howerver, such a simple feature lacks the discrimination power because it is often the case that similar arrangements of $f$ points are obtained from different feature points. In order to increase the discrimination power, we utilize feature points of a broader area. It is performed by increasing the number $m(>f)$. As $m$ increases, probability that different feature points have similar arrangement of $m$ points decreases. 
#### Registration

The index $H_{index}$ of the hash table is calculated by the following hash function:
$$
H_{index} = 
\left(
\sum_{i=0}^{mC_f-1} r_{(i)}k^i\right) \ \ mod\ \ H_{size}$$
where $r_{(i)}$ is the discrete value of the invariant, $k$ is the level of quantization of the invariant, and $H_{size}$ is the size of the hash table.

The algorithm of registration of mission pads to the database is shown below.

1:  **for each** $p\in\left\{ All\ \ feature\ \ points\ \ in\ \ a\ \ database\ \ image\right\}$ **do**
2: 　$P_n\leftarrow$ $The\ \ nearest\ \ n\ \ points\ \ of\ \ p (clockwise)$ 
3: 　**for each** $P_m \in \left\{All\ \ combinations\ \ of\ \ m\ \ points\ \ from\ \ P_n \right\}$ **do**
4: 　　**for each** $P_f \in \left\{All\ \ combinations\ \ of\ \ f\ \ points\ \ from\ \ P_m \right\}$ **do**
5: 　　　$r_{(i)} \leftarrow The\ \ invariant\ \ caculated\ \ with \ \ P_f$
6: 　　**end for**
7: 　　$H_{index}\leftarrow The\ \ hash\ \ index\ \ calculated\ \ by\ \ Eq.()$
8:　　 Register the item (document ID, point ID, $r_{(0)}$,...,$r_{mC_f-1}$) using $H_{index}$
9:　 **end for**
10: **end for**

#### Retrieval
The retrieval algorithm is shown below.
1:　**for each** $p \in \left\{All\ \ feature\ \ points\ \ in \ \ a\ \ query\ \ image \right\}$ **do**
2: 　　$P_n \leftarrow The\ \ nearest\ \ n\ \ points\ \ of\ \ p(clockwise)$
3: 　　**for each** $P_m \in \left\{All\ \ combinations\ \ of\ \ m\ \ points\ \ from\ \ P_n \right\}$ **do**
4: 　　　**for each** $P_m' \in \left\{Cyclic\ \ permutations of P_m \right\}$ **do**
5: 　　　　**for each** $P_f \in \left\{All\ \ combinations\ \ of\ \ f\ \ points from\ \ P_m'\right\}$**do**
6: 　　　　　$r_{(i)}\leftarrow The\ \ invariant\ \ calculated\ \ with\ \ P_f$
7: 　　　　**end for**
8: 　　　　$H_{index}\leftarrow The\ \ hash\ \ index\ \ calculated\ \ by\ \ Eq.(3)$
9: 　　　　Look up the hash table using $H_{index}$ and obtain the list.
10: 　　   　**for each** Item of the list **do**
11: 　　 　　**if** Conditions 1 to 3 are satisfied **then**
12: 　　　 　　Vote for the document ID in the voting table.
13: 　　　 　**end if**
14: 　　 　**end for**
15: 　　**end for**
16: 　**end for**
17: **end for**
18:Return the document image with the maximum votes.

In order to remove items with different sequences of invariants, the following condition is employed.
**Condition 1:** All values of $r_{(0)} ... r_{(mC_{}(f-1))}$ in the item are equal to those calculated at the lines 5 to 7 for $P_m'$
**Condition 2:** It is the first time to vote for the document ID with the point $p$.
**Condition 3:** It is the first time to vote for the point ID of the document ID.
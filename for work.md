
## CTM
### 1. paper 
https://www.spiedigitallibrary.org/conference-proceedings-of-spie/10148/1014809/Cost-effective-solution-using-inverse-lithography-OPC-for-DRAM-random/10.1117/12.2257857.full#_=_

```cpp
template <typename T>
struct RawPoint {
    T m_x;
    T m_y;
};

template <class Point>
struct Box {
    Point m_bottom_left_point;
    Point m_top_right_point;
};

struct RawCell {
enum class Orientation {
    NOT_SET = -1,
    R0 = 0,     
    R90 = 1,    
    MX = 2,     
    MXR90 = 3,  
    MY = 4,
    MYR90 = 5,
    MYMX = 6,
    MYMXR90 = 7
};
    using RawCellBox = Box<RawPoint<int64_t>>;

    long m_gds_pattern_index{-1};
    Orientation m_orientation = Orientation::R0;
    RawCellBox m_bbox;
    int m_priority{0};
};
```

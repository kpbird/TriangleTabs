TriangleTabs
============

** TriangleTabs is extension of PagerSlidingTabStrip **

```
Note: This is not a plug and play library that you can directly import in project and use. Triangle tabs is extension of PagerSlidingTabStrip library. In this article I will explain, how to modify PageSlidingTabStrip library to create Triangle Tabs.
```

**Android Triangle Tabs Video**

[![IMAGE ALT TEXT HERE](http://img.youtube.com/vi/2w8-wzr-wx8/0.jpg)](http://www.youtube.com/watch?v=2w8-wzr-wx8)

**Android Triangle Tabs Screenshot**

![Android Triangle Tabs](https://lh4.googleusercontent.com/KsK_vEvo-5pX8VrQSsUSuD4wfkNIJph0FidzUey3Xec=w278-h520-no)


## Customize PageSlidingTabStrip
Step 1: Import PageSlidingTabStrip library in AndroidStudio

Step 2: Let add new attribute for Triangle Tabs in attrs.xml (project -> res -> values). This attribute will allow developer to enable / disable triangle tabs.

```
<attr name="pstsTriangleIndicator" format="boolean"/>
```
Step 3: We need to modify Java code for Triangle Tabs.Goto source code and Open PagerSlidingTabStrip.java file

Step 4: Declare boolean variable above constructor, for pstsTriangleIndicator attribute.

```
private boolean triangleIndicator = false;
```

Step 5: Fatch pstsTriangleIndicator attribute, write following line in constructor

```
triangleIndicator = a.getBoolean(R.styleable.PagerSlidingTabStrip_pstsTriangleIndicator, triangleIndicator);
```

Step 6: Now it's time to draw triangle tab, goto onDraw method and add following code.

**Original Code**

```
canvas.drawRect(lineLeft, height - indicatorHeight, lineRight, height, rectPaint);
```

**New Code**, conditional code for triangle tabs

```
if (triangleIndicator) {
            Rect r = new Rect();
            int left = (int) lineLeft + (int) (((lineRight - lineLeft) / 2) - (indicatorHeight / 2)) - 20;
            int top = height - indicatorHeight;
            int right = (int) left + indicatorHeight + 30;
            int bottom = height;
            r.set(left, top, right, bottom);
            Path path = getEquilateralTriangle(r);
            canvas.drawPath(path, rectPaint);
        } else {
            canvas.drawRect(lineLeft, height - indicatorHeight, lineRight, height, rectPaint);
        }
```

Step 7: This is last step, declare getEquilateralTriangle() function. This function has code for triangle.

```
 public Path getEquilateralTriangle(Rect bounds) {
        Point startPoint = null, p2 = null, p3 = null;
        int width = bounds.right - bounds.left;
        int height = bounds.bottom - bounds.top;

        startPoint = new Point(bounds.left, bounds.bottom);

        p2 = new Point(startPoint.x + width, startPoint.y);
        //p3 = new Point(startPoint.x + (width / 2), startPoint.y - width);
        p3 = new Point(startPoint.x + (width / 2), startPoint.y - height);

        Path path = new Path();
        path.moveTo(startPoint.x, startPoint.y);
        path.lineTo(p2.x, p2.y);
        path.lineTo(p3.x, p3.y);

        return path;
    }
```

## Usage

Step 1: Include the customized library as local library project or you can copy PagerSlidingTabStrip.java, attrs.xml and background_tab.xml in respective folders

Step 2: Include the PagerSlidingTabStrip widget in your layout. This should usually be placed above the ViewPager it represents. Noticed pstsTriangleIndicator at

```
 <com.kpbird.triangletabs.PagerSlidingTabStrip
        android:id="@+id/activity_main_pagertabstrip"
        android:layout_width="match_parent"
        android:layout_height="45dp"
        custom:pstsTextAllCaps="true"
        custom:pstsIndicatorColor="#FF3F9FE0"
        custom:pstsDividerColor="#FF3F9FE0"
        custom:pstsUnderlineColor="#FF3F9FE0"
        custom:pstsTriangleIndicator="true"
        custom:pstsShouldExpand="true"
        custom:pstsIndicatorHeight="15dp"
/>

```


Step 3: In your onCreate method (or onCreateView for a fragment), bind the widget to the ViewPager.

```
// Declar variable 
private ViewPager pager;
private PagerSlidingTabStrip pagertab;
private MyPageAdapter pageAdapter;

// Inside onCreate
pager = (ViewPager) findViewById(R.id.activity_main_pager);
pagertab = (PagerSlidingTabStrip) findViewById(R.id.activity_main_pagertabstrip);
List<Fragment> fragments = getFragments();
pageAdapter = new MyPageAdapter(getSupportFragmentManager(), fragments);
pager.setAdapter(pageAdapter);
pagertab.setViewPager(pager);
```
Step 4: Compile & Run application







# OOC

In this quick guide we are going to learn about Object Oriented C programming.

![Memory](https://he-s3.s3.amazonaws.com/media/uploads/383f472.png)

---

## Pointer

```c
/**
* Value: stack
*/
int a;
printf("Address = %p", &a);
printf("Value = %d", a);

/**
* Address: stack
* Value: heap | stack
*/
int *a = NULL;
printf("Address = %p", a);
printf("Value = %d", *a);
```

---

## Function Pointer

```c
/**
* Value: text
*/
int fn (int x) {
    return x * x;
}
printf("Function Address = %p", fn);
```

### First Class Function (Save - Pass - Return)

![First Class Function](https://www.ocf.berkeley.edu/~shidi/cs61a/w/images/thumb/2/2f/Higherorderfunctions.JPG/500px-Higherorderfunctions.JPG)

```c
/**
 * Function save
 */
int fn (float x) {
    return x*x;
}
int (*myFn)(float) = fn;

/**
 * Function pass
 */
int doit (int (*fn)(int), int arg) {
    return fn(arg);
}

/**
 * Function return
 */
int fx (float x) {
    return x*x;
}

int(*)(float) fn () {
    return fx;
}
```

---

## Encapsulation (pack)

**shape.h**:

```c
/**
 * Define Class
 */
typedef struct Shape Shape;
struct Shape {
    int x;
    int y;
    void (*draw)(struct Shape* this);
};

/**
 * Define Constructor, Destructor
 */
Shape* shape_new();
void shape_free(Shape* shape);
```

**shape.c**:

```c
void draw(Shape* this) {
    printf("X: %d\nY: %d", this->x, this->y);
}

Shape* shape_new() {
    Shape* shape = malloc(sizeof(Shape));

    // Constructor body
    shape->x = 10;
    shape->y = 15;

    // Methods binding
    shape->draw = draw;

    return shape;
}
void shape_free(Shape* shape) {
    free(shape);
}
```

---

## Inheritance (super)

**rectangle.h**:

```c
typedef struct Shape Shape;
struct Shape {
    int x;
    int y;
};

typedef struct Rectangle Rectangle;
struct Rectangle {
    Shape super;

    int width;
    int height;
}

Rectangle* rectangle_new();
void rectangle_free(Rectangle* rectangle);
```

**rectangle.c**

```c
Rectangle* rectangle_new() {
    Rectangle* rectangle = malloc(sizeof(Rectangle));

    // Constructor body

    // Init super
    Shape* shape = (Shape*) rectangle;
    shape->x = 0;
    shape->y = 0;

    // Init this
    rectangle->width = 10;
    rectangle->height = 20;

    return rectangle;
}
```

---

## Polymorphism (override)

```c
typedef struct Shape Shape;
struct Shape {
    int x;
    int y;

    void (*draw)(Shape* this);
};

typedef struct Rectangle Rectangle;
struct Rectangle {
    Shape super;

    int width;
    int height;
}

Rectangle* rectangle_new();
void rectangle_free(Rectangle* rectangle);
```

**rectangle.c**

```c
Rectangle* rectangle_new() {
    Rectangle* rectangle = malloc(sizeof(Rectangle));

    // Constructor body

    // Init super
    Shape* shape = (Shape*) rectangle;
    shape->x = 0;
    shape->y = 0;
    shape->draw = oldDraw;

    // Init this
    rectangle->width = 10;
    rectangle->height = 20;
    // Overriding method
    shape->draw = newDraw;

    return rectangle;
}
```

#### Virtual Table

---

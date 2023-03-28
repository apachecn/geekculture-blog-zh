# 通过实现 AST 来比较 Rust、C++、Scala 和其他语言

> 原文：<https://medium.com/geekculture/comparing-rust-c-scala-and-other-languages-by-implementing-an-ast-888d09d31e01?source=collection_archive---------19----------------------->

抽象语法树(ASTs)是一种有趣的数据结构，因为它们的表示在不同的类型系统中变化很大。虽然我在这里专门写 AST，但在这篇文章中，AST 实际上是任何一种多态树的代理。对多态树结构建模是一回事，但是遍历所述树通常需要一组相互递归的函数(或者匹配一个函数的单个模式),这也很有趣。

上周我用几种不同的语言实现了一个超级简单的表达式 AST、C++、Elixir、Haskell、Java、Python、Rust、Scala 和 Zig。可以在这里找到回购:[https://github.com/mkhan45/expr_eval](https://github.com/mkhan45/expr_eval)。

一般来说，函数式语言的实现更符合习惯；下面的 Haskell 对任何 Haskell 新手来说都很容易理解，但是对那些只熟悉命令式语言的人来说可能很难理解。

```
data Op = Add | Sub | Mul | Div

data Expr
    = Atomic Int
    | Binary Op Expr Expr

eval :: Expr -> Int
eval (Atomic v) = v
eval (Binary Add lhs rhs) = (eval lhs) + (eval rhs)
eval (Binary Sub lhs rhs) = (eval lhs) - (eval rhs)
eval (Binary Mul lhs rhs) = (eval lhs) * (eval rhs)
eval (Binary Div lhs rhs) = (eval lhs) `div` (eval rhs)

main = do
    print $ eval (Binary Add (Atomic 3) (Binary Mul (Atomic 2) (Atomic 5)))
```

另一方面，我认为下面的 Scala 对于任何有一点 OOP 知识的开发人员来说都很容易理解。尽管语法不同，但它在语义上几乎与 Haskell 相同。

```
abstract class BinOp
case object Add extends BinOp
case object Sub extends BinOp
case object Mul extends BinOp
case object Div extends BinOp

abstract class Expr
case class AtomicExpr(value: Int) extends Expr
case class BinaryExpr(op: BinOp, lhs: Expr, rhs: Expr) extends Expr

object Main {
    def eval(expr: Expr): Int = {
        expr match {
            case AtomicExpr(value) => value

            case BinaryExpr(op, lhs, rhs) =>
                op match {
                    case Add => eval(lhs) + eval(rhs)
                    case Sub => eval(lhs) - eval(rhs)
                    case Mul => eval(lhs) * eval(rhs)
                    case Div => eval(lhs) / eval(rhs)
                }
        }
    }

    def main(args: Array[String]) = {
        println(eval(BinaryExpr(Add, AtomicExpr(1), AtomicExpr(2))))
    }
}
```

接下来，我们将看看 Java 版本，它使用抽象类来模拟 sum 类型。我已经从代码片段中删除了构造函数，以消除一些干扰。

```
enum BinOp { Add, Sub, Mul, Div }

public abstract class Expr {
    abstract int eval();

    static class AtomicExpr extends Expr {
        int val;

        int eval() {
            return val;
        }
    }

    static class BinaryExpr extends Expr {
        BinOp op;
        Expr lhs;
        Expr rhs;

        int eval() {
            return switch (op) {
                case Add -> lhs.eval() + rhs.eval();
                case Sub -> lhs.eval() - rhs.eval();
                case Mul -> lhs.eval() * rhs.eval();
                case Div -> lhs.eval() / rhs.eval();
            };
        }
    }

    public static void main(String[] args) {
        Expr e = new BinaryExpr(BinOp.Mul, new AtomicExpr(5), new AtomicExpr(10));
        System.out.println(e.eval());
    }
}
```

看看 Java 就明白了为什么 Scala 的 sum 类型实现使用抽象类。它带有 Java 通常的冗长，但相对容易使用。添加新的类和构造函数等。因为每一种新的节点都会很快老化。

[表达式计算器的 Rust 版本](https://github.com/mkhan45/expr_eval/blob/main/rust/expr.rs)和 Scala 几乎一样，这里就不包含代码了。因为 sum 类型是第一类，所以声明和遍历树都非常符合人类工程学。然而，由于缺少垃圾收集器，处理引用要冗长得多，并且由于生存期或盒子的原因，增加了许多语法上的干扰。虽然速度很快，但接下来让我们看看 C、C++和 Zig。

我将从 c 开始。实现比以前的长得多，如果不一定更难写的话，所以我在下面的代码片段中删除了一些不太相关的部分。我还将枚举定义重新格式化为一行。完整档案在这里:【https://github.com/mkhan45/expr_eval/blob/main/c/expr.c】T4。

```
typedef enum expr_ty { Atomic, Binary } expr_ty;

typedef enum bin_op { Add, Sub, Mul, Div } bin_op;

typedef struct atomic_expr {
    int val;
} atomic_expr_t;

typedef struct binary_expr {
    enum bin_op op;
    struct expr* lhs;
    struct expr* rhs;
} binary_expr_t;

typedef struct expr {
    expr_ty ty;
    union {
        atomic_expr_t atomic;
        binary_expr_t* binary;
    };
} expr_t;

int eval_binary_expr(const binary_expr_t* expr) {
    switch (expr->op) {
        case Add: return eval_expr(expr->lhs) + eval_expr(expr->rhs);
        case Sub: return eval_expr(expr->lhs) - eval_expr(expr->rhs);
        case Mul: return eval_expr(expr->lhs) * eval_expr(expr->rhs);
        case Div: return eval_expr(expr->lhs) / eval_expr(expr->rhs);
    }
}

int eval_expr(const expr_t* expr) {
    switch (expr->ty) {
        case Atomic: return expr->atomic.val;
        case Binary: return eval_binary_expr(expr->binary);
    }
}

int main() {
    expr_t* expr = new_binary(Add, new_atomic(12), new_binary(Mul, new_atomic(3), new_atomic(5)));
    printf("%d\n", eval_expr(expr));
    free_expr(expr);
}
```

这段代码使用标记的 union 模式来模拟适当的 sum 类型。这很容易解释，尽管你必须确保正确管理你的内存。能够在没有模式匹配的情况下访问联合的数据是一把双刃剑；如果你搞砸了，坏事就会发生。实际上，这是 c 语言中影响最小的安全问题之一。

Zig 版本几乎是相同的，除了 Zig 有一些语言特性，使标记的联合更符合人体工程学。这使得它在视觉上与 Rust 版本非常相似，即使它在语义上与 C 版本相同。

```
const std = @import("std");

const BinOp = enum { add, sub, mul, div };

const ExprTy = enum { atomic, binary };

const BinaryExpr = struct {
    op: BinOp,
    lhs: *const Expr,
    rhs: *const Expr,
};

const Expr = union(ExprTy) {
    atomic: isize,
    binary: BinaryExpr,
};

fn eval(expr: *const Expr) isize {
    return switch (expr.*) {
        ExprTy.atomic => |v| v,
        ExprTy.binary => |b| {
            const lhs = eval(b.lhs);
            const rhs = eval(b.rhs);
            return switch (b.op) {
                BinOp.add => lhs + rhs,
                BinOp.sub => lhs - rhs,
                BinOp.mul => lhs * rhs,
                BinOp.div => @divFloor(lhs, rhs),
            };
        }
    };
}

pub fn main() !void {
    const stdout = std.io.getStdOut().writer();

    const expr = Expr { 
        .binary = BinaryExpr { 
            .op = BinOp.add,
            .lhs = &Expr { 
                .binary = BinaryExpr {
                    .op = BinOp.mul,
                    .lhs = &Expr { .atomic = 5 },
                    .rhs = &Expr { .atomic = 3 }
                }
            },
            .rhs = &Expr { .atomic = 2 },
        }
    };

    try stdout.print("{d}\n", .{eval(&expr)});
}
```

这是我在 Hello World 之后的第一个 Zig 程序，我很惊讶我有多喜欢它。定义实际的多态树结构仍然需要多个`struct`定义，这对于更复杂的 ASTs 来说有点笨拙，但是遍历树是非常好的。

最后，我们来看看 C++。就 C++而言，有三种方法可以做到这一点，我不确定哪一种被认为是最惯用的。

第一种方法是在 C 之后使用标记联合对其建模。这种方法具有 C 实现的大部分缺点，但是由于智能指针，它可以显著地更加安全。在我看来，这是在 C++中最好的方法。

第二种方法是在 Java 之后使用抽象类对其建模。C++实际上没有抽象类，但是它有虚方法和继承，所以已经很接近了。这就是`clang`编译器对其 AST 建模的方式。

```
enum BinOp { Add, Sub, Mul, Div };

struct OhNo {};

class Expr {
    public:
        virtual int eval() const { throw OhNo{}; };
        virtual ~Expr() = default;
};

class AtomicExpr : public Expr {
    public:
        int val;

        AtomicExpr(const int val) { this->val = val; }
        int eval() const override {
            return this->val;
        }
};

class BinaryExpr : public Expr {
    public:
        BinOp op;
        std::unique_ptr<Expr> lhs;
        std::unique_ptr<Expr> rhs;

        BinaryExpr(const BinOp op, Expr* lhs, Expr* rhs) {
            this->op = op;
            this->lhs = std::unique_ptr<Expr>(lhs);
            this->rhs = std::unique_ptr<Expr>(rhs);
        }

        int eval() const override {
            switch (this->op) {
                case Add: return lhs->eval() + rhs->eval();
                case Sub: return lhs->eval() - rhs->eval();
                case Mul: return lhs->eval() * rhs->eval();
                case Div: return lhs->eval() / rhs->eval();
                default: throw "Unimplemented";
            }
        }
};

int main() {
    const Expr* e = new BinaryExpr(Mul, new AtomicExpr(5), new BinaryExpr(Add, new AtomicExpr(3), new AtomicExpr(1)));
    std::cout << e->eval() << std::endl;
    delete e;
}
```

这段代码在语义上与 Java 相同。有点啰嗦但不比 Rust 差。然而，它充满了陷阱。

您可能已经注意到了`eval()`的默认实现中的`throw OhNo {}`。因为 C++没有抽象类，基类`Expr`可能被实例化，这是非法的。部分解决这个问题的一个方法是将`AtomicExpr`作为基类，但这并不十分直观。这里的另一个陷阱是 C++中的方法分派有些棘手。被覆盖的方法必须通过指针来调用，以使覆盖真正工作，因此很容易编写代码，在应该调用`BinaryExpr::eval`或`AtomicExpr::eval`时错误地调用`Expr::eval`。

用 C++实现这个程序的最后一种方法是使用`std::variant`。`[std::variant](https://bitbashing.io/std-visit.html)` [时机不好](https://bitbashing.io/std-visit.html)。它看起来是这样的:

```
enum BinOp { Add, Sub, Mul, Div };

struct AtomicExpr;
struct BinaryExpr;

using Expr = std::variant<AtomicExpr, BinaryExpr>;

struct AtomicExpr { 
    int val; 
    AtomicExpr(int val) : val(val) {}
};

struct BinaryExpr { 
    BinOp op;
    std::shared_ptr<Expr> lhs;
    std::shared_ptr<Expr> rhs;

    BinaryExpr(BinOp op, Expr lhs, Expr rhs) {
        this->op = op;
        this->lhs = std::make_shared<Expr>(lhs);
        this->rhs = std::make_shared<Expr>(rhs);
    }
};

int eval(const Expr& e);

struct EvalVisitor {
    int result;

    void operator()(const AtomicExpr& e) {
        result = e.val;
    }

    void operator()(const BinaryExpr& e) {
        int lhs = eval(*e.lhs);
        int rhs = eval(*e.rhs);

        switch (e.op) {
            case Add: result = lhs + rhs; break;
            case Sub: result = lhs - rhs; break;
            case Mul: result = lhs * rhs; break;
            case Div: result = lhs / rhs; break;
        }
    }
};

int eval(const Expr& e) {
    auto visitor = EvalVisitor { 0 };
    std::visit(visitor, e);
    return visitor.result;
}

int main() {
    Expr e = BinaryExpr(Add, AtomicExpr(3), BinaryExpr(Mul, AtomicExpr(5), AtomicExpr(8)));
    std::cout << eval(e) << std::endl;
}
```

构建实际的变体结构非常容易。用`std::visit`遍历就不是了。不使用 Visitor struct，我可以使用模板和动态闭包，但是，不管怎样，sum 类型显然不是 C++的强项。使用`std::variant`时的错误信息难以理解。

总体而言，有三个主要类别:

1.  第一类和类型

*   这包括函数式语言、Haskell、Scala 和 Rust。
*   这种方式绝对是最符合人体工程学的。

2.抽象类/多态性

*   这主要是 Java，尽管 Scala 在某种程度上也算。还有 C++，只不过使用虚拟方法的 C++版本比简单的标记联合更不符合人体工程学。

3.标记的联合

*   这是 C 和 Zig 的方法，也是我喜欢的 C++方法。

然后是`std::variant`，它试图像一流的一些类型一样好，但悲惨地失败了。

值得注意的是，我没有写我的 Python 或 Elixir 实现。因为它们是动态类型的，所以很容易对多态树建模。问题是没有编译时安全性，但这与动态语言没有什么不同。

我认为很明显，具有一级 sum 类型的函数式语言更适合建模 ast。然而，Scala 和 Rust 都将适当的代数数据类型实现成了命令式风格；求和类型所必需的唯一其他函数式语言特性是模式匹配，它也非常适合命令式语言。我期待当今任何现代语言中的一流和类型。也就是说，Zig 的替代方案非常有效。它符合 Zig 不隐藏实现细节的目标，并且只放弃最低限度的安全保证。

*原载于 2021 年 5 月 10 日*[*https://mkhan 45 . github . io*](https://mkhan45.github.io/2021/05/10/Modeling-ASTs-in-Different-Languages.html)*。*

*在 Twitter 上关注我:*[*twitter.com/fiiissshh*](http://twitter.com/fiiissshh)
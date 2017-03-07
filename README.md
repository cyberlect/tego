# Tego
**Tego** is a language for term rewriting.

## Example
You can define your own constructors:

	define MyCons(x: Int, y: Bool, z: String);

Types are optional (default to `Term`):

	define MyCons(x, y, z);
	
Constructors can inherit from other constructors.

	define Expr();
	define BinExpr(left: Expr, right: Expr) extends Expr;
	define Plus(left: Expr, right: Expr) extends BinExpr;

The following term types are pre-defined:

- Int
- Bool
- Char
- String
- List
- Term
- Some
- None
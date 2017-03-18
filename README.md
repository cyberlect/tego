[![AppVeyor](https://img.shields.io/appveyor/ci/Virtlink/tego/master.svg)](https://ci.appveyor.com/project/Virtlink/tego)
[![GitHub release](https://img.shields.io/github/release/Cyberlect/tego.svg)](https://github.com/Cyberlect/tego/releases)
[![NuGet](https://img.shields.io/nuget/v/Tego.svg)](https://www.nuget.org/packages/Tego/)
[![GitHub license](https://img.shields.io/github/license/Cyberlect/tego.svg)](http://www.apache.org/licenses/LICENSE-2.0)
[![Maintenance](https://img.shields.io/maintenance/yes/2017.svg)](https://github.com/Cyberlect/tego/commits/master)

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


## Installation
Easiest is to install the NuGet package.

```PowerShell
PM> Install-Package Yargon.ATerms
```

## License
Copyright 2016, 2017 - Daniel Pelsmaeker

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at <http://www.apache.org/licenses/LICENSE-2.0>. Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.

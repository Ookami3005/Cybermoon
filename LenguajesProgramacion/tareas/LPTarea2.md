[<- Volver](../LenguajesProgramacion.md)
#### Atómicos

`<Var>` := a | b | c | ... | z

`<Sign> :=` + | -
`<N> :=` 0 | 1 | 2 | ... | 9
`<Z> :=` 1 | 2 | ... | 9
`<Int> :=` `[<Sign>]` `<Z>` `{<N>}` | 0

`<Bool> :=` true | false

#### Operadores numéricos

`<CompSymb> :=` > | <
`<ArithExp> :=` ( `<ArithExp>` `<Sign>` `<ArithExp>` ) | `<Var>` | `<Int>`
`<CompExp> :=` ( `<CompExp>` `<CompSymb>` `<CompExp>` ) | `<Var>` | `<Int>`

#### Operadores lógicos

`<LogSymb> :=`  $\land$ | $\lor$
`<LogExp> :=` ( $\neg$ `<LogExp>` ) | ( `<LogExp>` `<LogSymb>` `<LogExp>` ) | `<RelOpExp>` | `<CompExp>` | `<Bool>`

#### Operadores relacionales

`<RelOpExp> :=` ( `<LogExp>` == `<LogExp>` ) | ( `<ArithExp>` == `<ArithExp>` )

#### Secuencias de control

`<IF> :=`
	if (`<LogExp>`) {
		`{<SimpleExp> "\n"}` }

`<WHILE> :=`
	while (`<LogExp>`) {
		`{<SimpleExp> "\n"}` }
#### Expresión básica del lenguaje

`<SimpleExp> :=` `[<Var> = ]` `<LogExp>` ; | `[<Var> = ]` `<ArithExp>` | print(`<LogExp>`); | print(`<ArithExp>`) | `<IF>` | `<WHILE>`;

## Codigo completo

`<S> := {<SimpleExp> "\n"}`
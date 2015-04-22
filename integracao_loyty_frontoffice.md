
# Documentação de Integração com Loyty FrontOffice

## Introdução

O módulo de FrontOffice fornecido pela Loyty destina-se à integração de soluções ERP com a plataforma de
Loyty API. O Loyty FrontOffice é um conjunto de ficheiros (dlls e ficheiros de
configuração) que deveram ser instalados nos PC’s onde corre a aplicação de facturação. Este
módulo funciona como um integrador entre as duas plataformas, estando toda a plataforma da Loyty API
alojada remotamente. O sistema desenvolvido funciona com o pressuposto de existência de ligação permanente entre o PC e o Servidor onde estará alojado a Loyty API. 

## Instalação

**Pré-Requisitos**:
* Windows (Mínimo) XP
* Framework .NET 4.5
* Ligação de rede ao ambiente Loyty API

Para utilizar o módulo de FrontOffice Loyty é necessário registar a DLL “loytyPOS.dll”.
O registo é efectuado através do comando:

	REGASM /CODEBASE C:\___DIR____\loytyPOS.dll

Se a plataforma de desenvolvimento for .NET, não é necessário o registo das dlls, sendo apenas
necessário a DLL estar incluída no projecto.

## Interface

O módulo de FrontOffice Loyty disponibiliza quatro métodos que permitem fazer a integração de informação:

**IdentificarCliente** – para identificar cliente de uma venda, este deve ser invocado no momento que se inicia  a venda (mas não é obrigatório ser no inicio, deve ser feito antes do PreRegisto). Este método lança uma janela com dados estatísticos do cliente, e devolve um conjunto de strings que pode ser utilizado, por exemplo, indicação do número de cliente no ERP ou para impressão.

**FichadeCliente** – para lançar uma janela de consulta do cliente. Esta componente irá permitir gerir os dados do cliente, consulta de vendas, saldo em cartão. Este método devolve conjunto de strings para com os mesmo dados apresentados na janela.

**PreRegisto** – para registo de uma venda simulada, este tem de ser invocado no momento imediatamente antes da conta ser fechada, pois pode ainda receber a indicação de um desconto. Este método devolve o desconto a aplicar ( caso exista ), e o valor em cartão que tenha sido marcado a utilizar.

**FecharCompra** – para registo de uma venda efectiva, este tem de ser invocado no momento em que a
conta é fechada, mas ainda pode receber a indicação de um desconto. Este método devolve o desconto a aplicar ( caso exista ), um conjunto de strings para impressão e vales que tenham sido resgatados.

## Descrição técnica

### Funções da DLL
#### IdentificarCliente(string input)
O argumento de input deve ser uma string com o formato de XML (ver schema _IdentificarClienteDLL.xsd_).

```xml
<?xml version="1.0" encoding="UTF-8"?>
<input>
  <sTexto>Rolex</sTexto>
  <CodLoja />
  <sParametros>nome</sParametros>
  <xmlParceria />
</input>
``` 

Esta função retorna um XML com o resultado da operação.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<output>
  <CLIENTES>
    <CLIENTE IDCLIENTE="6" NUMERO="10020197" NOME="Rita Rolex" SEXO="F" MORADA="Av Eng. Arantes E Oliveira 6 - 7b" CODPOSTAL="Lisboa" CODPOST3="222" CODPOST4="1900" CODIGOPOSTAL="1900-222" DOCUMENTOIDENTIFICACAO="" DESIGNACAOPOSTAL="Lisboa" LOCALIDADE="Lisboa" TELEFONE="351218407717" CODIGOIDENTIFICACAO="" DIASPRIMEIRACOMPRA="468" NIF="" BI="" TELEMOVEL="" EMAIL="francisco.esteves@eisa.pt" OBS=" " PONTOS="" VALORCARTAO="82.00" SOPARAVALOR="" VALORCOMPRASFALTA="7083.35" PASSOUCARTAO="" DADOSESTATISTICOS="Últ. Compra: 17-04-2015 12:02:44 | R - 5 F - 5 M - 5" DATANASCIMENTO="" INFOCLIENTECAB="Rita Rolex - " INFOCLIENTEDET="5 - 5 - 5 | 17-04-2015 12:02:44" />
    <MENSAGENS />
  </CLIENTES>
</output>
```

#### FichadeCliente(string input)
O argumento de input deve ser uma string com o formato de XML (ver schema _FichaDeClienteDLL.xsd_).
```xml
<?xml version="1.0" encoding="UTF-8"?>
<input>
  <sTexto>Rolex</sTexto>
  <CodLoja />
  <sParametros>nome</sParametros>
  <xmlParceria />
  <iUltimosMeses>0</iUltimosMeses>
  <iUltimasCompras>0</iUltimasCompras>
  <bDetalhados>false</bDetalhados>
  <dataDe>1900-01-01</dataDe>
  <dataA>2014-07-19</dataA>
  <tipo />
</input>
```

Esta função retorna um XML com o resultado da operação.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<output>
  <CLIENTES>
    <CLIENTE IDCLIENTE="328494" NUMERO="100003" NOME="Cliente Teste 2" SEXO="I" MORADA="" CODPOSTAL="" CODPOST3="" CODPOST4="" CODIGOPOSTAL="" DOCUMENTOIDENTIFICACAO="" DESIGNACAOPOSTAL="" LOCALIDADE="" TELEFONE="" CODIGOIDENTIFICACAO="" DIASPRIMEIRACOMPRA="0" NIF="" BI="" TELEMOVEL="" EMAIL="" OBS="" PONTOS="" VALORCARTAO="" SOPARAVALOR="1249.43" VALORCOMPRASFALTA="9" PASSOUCARTAO="" DADOSESTATISTICOS="Últ. Compra: 17-04-2015 11:53:40 | R - 0 F - 0 M - 0" DATANASCIMENTO="" INFOCLIENTECAB="Cliente Teste 2 - " INFOCLIENTEDET="0 - 0 - 0 | 17-04-2015 11:53:40" />
    <MENSAGENS />
  </CLIENTES>
  <CARTOES />
  <COMPRAS>
    <COMPRA LojaDescricao="Loja Desenvolvimento EISA" Total="9" NumTalao="120" IdCartao="0" DataHoraTalao="2015-04-17T11:53:40" MeioLiquidacao="Numerário" Devolucao="0">
      <LINHA Valor="217" Desconto="0" OperadorDescricao="teste" IdCompra="473461" Quantidade="2" ArtigoCodigo="XPU006B/PU009A    " ArtigoDescricao="CASACO            " CorDescricao="" TamanhoDescricao="" CampoAdicional1="" CampoAdicional2="" CampoAdicional3="" CampoAdicional4="" CampoAdicional5="" />
      <LINHA Valor="10" Desconto="1" OperadorDescricao="Operador Teste" IdCompra="515523" Quantidade="1" ArtigoCodigo="CVR0021/053       " ArtigoDescricao="100% Pele         " CorDescricao="" TamanhoDescricao="" CampoAdicional1="" CampoAdicional2="" CampoAdicional3="" CampoAdicional4="" CampoAdicional5="" />
    </COMPRA>
  </COMPRAS>
  <BENEFICIOS>
    <DINHEIRO VALOR="0" />
    <PONTOS VALOR="0" />
  </BENEFICIOS>
  <BENEFICIOS_CADUCADOS />
  <TICKETS />
</output>
```

#### PreRegisto(string input)
O argumento de input deve ser uma string com o formato de XML (ver schema _PreRegistoDLL.xsd_).

```xml
<?xml version="1.0" encoding="UTF-8"?>
<input>
  <sIdCliente>6</sIdCliente>
  <CodLoja>001</CodLoja>
  <xmlListaCompras>
    <COMPRAS VALDESCONTOTALAO="0">
      <COMPRA CODARTIGO="123" CODCOR="3" CODGRELHA="4" CODOPERADOR="2" DATAHORATALAO="2014-07-18T04:15:53" DESCONTO="0" DEVOLUCAO="0" MEIOLIQUIDACAO="" NUMTALAO="0" OFERTA="0" PERCIVA="23" QUANTIDADE="5" VALUNITARIO="123.00" />
    </COMPRAS>
  </xmlListaCompras>
  <dtDataTalao>2014-07-19T12:12:12</dtDataTalao>
  <sTipoCompra>S</sTipoCompra>
  <bPassouCartao>true</bPassouCartao>
  <xmlParcerias>
    <PARCERIAS>
      <PARCERIA CARTAOPARCERIA="1" CODPARCERIA="1">PARCERIA1</PARCERIA>
    </PARCERIAS>
  </xmlParcerias>
</input>
```

Esta função retorna um XML com o resultado da operação.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xmlBeneficios>
  <BENEFICIOS>
    <BENEFICIO IDBENEFICIO="" VALOR="10" PERC="0" PONTOS="" USADO="" TIPO="D" />
  </BENEFICIOS>
</xmlBeneficios>
```

#### FecharCompra(string input)
O argumento de input deve ser uma string com o formato de XML (ver schema _FecharCompraDLL.xsd_).
 
```xml
<?xml version="1.0" encoding="UTF-8"?>
<input>
  <sIdCliente>6</sIdCliente>
  <CodLoja>001</CodLoja>
  <xmlListaCompras>
    <COMPRAS VALDESCONTOTALAO="0">
      <COMPRA CODARTIGO="123" CODCOR="3" CODGRELHA="4" CODOPERADOR="2" DATAHORATALAO="2015-04-17T04:33:15" DESCONTO="0" DEVOLUCAO="0" MEIOLIQUIDACAO="" NUMTALAO="0" OFERTA="0" PERCIVA="23" QUANTIDADE="5" VALUNITARIO="123.00" />
    </COMPRAS>
  </xmlListaCompras>
  <dtDataTalao>2015-04-17T04:33:15</dtDataTalao>
  <sTipoCompra>E</sTipoCompra>
  <bPassouCartao>true</bPassouCartao>
  <xmlBeneficios>
    <BENEFICIOS>
      <BENEFICIO IDBENEFICIO="" PERC="5" PONTOS="" TIPO="DDP" USADO="" VALOR="30.75" />
      <BENEFICIO IDBENEFICIO="" PERC="0" PONTOS="" TIPO="D" USADO="" VALOR="2" />
    </BENEFICIOS>
  </xmlBeneficios>
  <xmlParcerias>
    <PARCERIAS>
      <PARCERIA CARTAOPARCERIA="1" CODPARCERIA="1" />
    </PARCERIAS>
  </xmlParcerias>
</input>
```

Esta função retorna um XML com o resultado da operação.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<output>
  <BENEFICIOS_ATRIBUIDOS>
    <BENEFICIO DESCRICAO="Valor" TIPO="D" VALOR="10" PONTOS="0" NUMTALAO="0" CODBARRAS="" IDCAMPANHA="1067" DESCRICAOCAMPANHA="Base" DATAINI="2015-04-17" DATAFIM="2015-07-17" PERC="0" IDLOJADESCONTARLIST="" DESCONTAVELNOUTRASLOJAS="" TIPOLOJASADERENTES="T" VALORMAXDESCONTO="0" VALORMINPROXCOMPRA="0" USADO=""></BENEFICIO>
  </BENEFICIOS_ATRIBUIDOS>
  <MENSAGENS />
  <BENEFICIOS>
    <DINHEIRO VALOR="90" />
    <PONTOS VALOR="0" />
  </BENEFICIOS>
  <COMUNICACOES>
    <COMUNICACAO EMAIL="" EmailAssunto="0" SMS="" />
  </COMUNICACOES>
  <TOTAIS VALORCARTAO="90.00" VALORTOTALGANHO="1410.00" />
  <COMPRA NUMTALAO="0" IDLOJA="0" DATAHORATALAO="2015-04-17 04:33:15" />
</output>
```

### Utilização da DLL em C\# 
 
```C#	
internal object DinamicallyCallDll(string methodName, string arguments)
{
    try
    {
        // Locate DLL, read method and its arguments
        Assembly assembly = Assembly.LoadFrom(DLL_PATH);
        Type type = assembly.GetType(DLL_NAMESPACE);
        Object o = Activator.CreateInstance(type);
        MethodInfo method = type.GetMethod(methodName);        // Prepare arguments for method
        ParameterInfo[] parameters = method.GetParameters();
        object[] methodParameters = new object[parameters.GetLength(0)];
        for (int i = 0; i < parameters.Length; i++)
        {
            var converter = TypeDescriptor.GetConverter(parameters[i].ParameterType);
            methodParameters[i] = converter.ConvertFrom(arguments);
        }        // Call method
        return method.Invoke(o, methodParameters);
    }
    catch (Exception error)
    {
        if (error is NullReferenceException)
            MessageBox.Show("Ocorreu um erro ao invocar o método da DLL:\n\"" + "O método " + methodName + " não existe.");
        else
            MessageBox.Show("Ocorreu um erro ao invocar o método da DLL:\n\"" + error.Message + error.InnerException + "\"");
        return "";
    }
}
```

Deve conter duas variáveis:
**DLL_PATH** 
Caminho absoluto onde se encontra a DLL.
**DLL_NAMESPACE_NAME**
Deve estar preenchida com a seguinte string “loytyPOS.dll”.

#### Argumentos
**methodName** deve indicar que operação pretende realizar:
•	IdentificarCliente
•	FichadeCliente
•	PreRegisto
•	PosRegisto
**arguments** para indicar qual o argumento que vai ser enviado para a operação desejada.
Esta função retorna um objeto do tipo **XMLDocument**.

#### Demo exemplo à chamada da DLL

Tendo uma janela de exemplo cria-se um método para o evento do botão “Identificar Cliente” que irá executar o código demonstrado: 

```C#
private void btnIdentificarCliente_Click(object sender, EventArgs e)
{
	string methodName = "IdentificarCliente";
	string arguments =  @"<?xml version="1.0" encoding="UTF-8"?>" +
						@"<input>" +
						@"  <sTexto>Rolex</sTexto>" +
						@"  <CodLoja />" +
						@"  <sParametros>nome</sParametros>" +
						@"  <xmlParceria />" +
						@"</input>";

	XmlDocument result = 
					(XmlDocument)DinamicallyCallDll(methodName, arguments);
					
	MessageBox.Show(result.OuterXml);	
}
```

### Utilização da DLL em Delphi 

Declaração da função externa
``` Delphi
function IdentificarCliente(input: PWideChar): PWideChar; stdcall; external 'loytyPOS.dll';
```
Chamada da função externa
``` Delphi
procedure TfrmIdentificarCliente.btnOkClick(Sender: TObject);
var
	sXML, sInput : WideString;
begin

	sInput := '<?xml version="1.0" encoding="UTF-8"?>' +
			  '<input>' +
			  '  <sTexto>Rolex</sTexto>' +
			  '  <CodLoja />' +
			  '  <sParametros>nome</sParametros>' +
			  '  <xmlParceria />' +
			  '</input>';
			
	sXML := IdentificarCliente(PWideChar(sInput));'loytyPOS.dll';
        
end;
```

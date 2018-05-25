Varchar往往用来保存可变长度的字符串。简单的说，我们只是给其固定了一个最大值，然后系统会根据实际存储的数据量来分配合适的存储空间。为 此相比CHAR字符数据而言，其能够比固定长度类型占用更少的存储空间。不过在实际工作中，由于某系特殊的原因，会在这里设置例外。如管理员可以根据需要 指定ROW_FORMAT=FIXED选项。利用这个选项来创建MyISAM表的话，系统将会为每一行使用固定长度的空间。此时会造成存储空间的损耗。通 常情况下，VARCHAR数据类型能够节约磁盘空间，为此往往认为其能够提升数据库的性能。不过这里需要注意的是，这往往是一把双刃剑。其在提升性能的同 时，往往也会产生一些副作用。如因为其长度是可变的，为此在数据进行更新时可能会导致一些额外的工作。如在更改前，其字符长度是10位（Varchar规 定的最长字符数假设是50位），此时系统就只给其分配10个存储的位置（假设不考虑系统自身的开销）。更改后，其数据量达到了20位。由于没有超过最大 50位的限制，为此数据库还是允许其存储的。只是其原先的存储位置已经无法满足其存储的需求。此时系统就需要进行额外的操作。如根据存储引擎不同，有的会 采用拆分机制，而有的则会采用分页机制。

CHAR数据类型与VARCHAR数据类型不同，其采用的是固定长度的存储方式。简单的说，就是系统总为其分配最大的存储空间。当数据保存时，即使 其没有达到最大的长度，系统也会为其分配这么多的存储空间。显然，这种存储方式会造成磁盘空间的浪费。这里笔者需要提醒的一点是，当字符位数不足时，系统 并不会采用空格来填充。相反，如果在保存CHAR值的时候，如果其后面有空值，系统还会自动过滤其空格。而在进行数据比较时，系统又会将空格填充到字符串 的末尾。

显然，VARCHAR与CHAR两种字符型数据类型相比，最大的差异就是前者是可变长度，而后者则是固定长度。在存储时，前者会根据实际存储的数据 来分配最终的存储空间。而后者则不管实际存储数据的长度，都是根据CHAR规定的长度来分配存储空间。这是否意味着CHAR的数据类型劣于VARCHAR 呢？其实不然。否则的话，就没有必要存在CHAR字符类型了。虽然VARCHAR数据类型可以节省存储空间，提高数据处理的效率。但是其可变长度带来的一 些负面效应，有时候会抵消其带来的优势。为此在某些情况下，还是需要使用Char数据类型。


根据上面的分析，我们知道VARCHAR数据类型是一把双刃剑，其在带来性能提升的同时，也可能会存在着一些额外的消耗。我们在评估到底是使用VARCHAR数据类型还是采用CHAR数据类型时，就需要进行均衡。在实际项目中，我们会考量如下情况。

一是根据字符的长度来判断。如某个字段，像人的名字，其最长的长度也是有限的。如我们给其分配18个字符长度即可。此时虽然每个人的名字长度有可能 不同，但是即使为其分配了固定长度的字符类型，即18个字符长度，最后浪费的空间也不是很大。而如果采用NVARCHAR数据类型时，万一以后需要改名， 而原先的存储空间不足用来容纳新的值，反而会造成一些额外的工作。在这种情况下，进行均衡时，会认为采用CHAR固定长度的数据类型更好。在实际项目中， 如果某个字段的字符长度比较短此时一般是采用固定字符长度。

二是考虑其长度的是否相近。如果某个字段其长度虽然比较长，但是其长度总是近似的，如一般在90个到100个字符之间，甚至是相同的长度。此时比较 适合采用CHAR字符类型。比较典型的应用就是MD5哈希值。当利用MD5哈希值来存储用户密码时，就非常使用采用CHAR字符类型。因为其长度是相同 的。另外，像用来存储用户的身份证号码等等，一般也建议使用CHAR类型的数据。

另外请大家考虑一个问题，CHAR（1）与VARCHAR（1）两这个定义，会有什么区别呢？虽然这两个都只能够用来保存单个的字符，但是 VARCHAR要比CHAR多占用一个存储位置。这主要是因为使用VARCHAR数据类型时，会多用1个字节用来存储长度信息。这个管理上的开销CHAR 字符类型是没有的。

三是从碎片角度进行考虑。使用CHAR字符型时，由于存储空间都是一次性分配的。为此某个字段的内容，其都是存储在一起的。单从这个角度来讲，其不 存在碎片的困扰。而可变长度的字符数据类型，其存储的长度是可变的。当其更改前后数据长度不一致时，就不可避免的会出现碎片的问题。故使用可变长度的字符 型数据时，数据库管理员要时不时的对碎片进行整理。如执行数据库导出导入作业，来消除碎片。

四是即使使用Varchar数据类型，也不能够太过于慷慨。这是什么意思呢？如现在用户需要存储一个地址信息。根据评估，只要使用100个字符就可 以了。但是有些数据库管理员会认为，反正Varchar数据类型是根据实际的需要来分配长度的。还不如给其大一点的呢。为此他们可能会为这个字段一次性分 配200个字符的存储空间。这VARCHAR（100）与VARCHAR（200）真的相同吗？结果是否定的。虽然他们用来存储90个字符的数据，其存储 空间相同。但是对于内存的消耗是不同的。对于VARCHAR数据类型来说，硬盘上的存储空间虽然都是根据实际字符长度来分配存储空间的，但是对于内存来 说，则不是。其时使用固定大小的内存块来保存值。简单的说，就是使用字符类型中定义的长度，即200个字符空间。显然，这对于排序或者临时表（这些内容都 需要通过内存来实现）作业会产生比较大的不利影响。所以如果某些字段会涉及到文件排序或者基于磁盘的临时表时，分配VARCHAR数据类型时仍然不能够太 过于慷慨。还是要评估实际需要的长度，然后选择一个最长的字段来设置字符长度。如果为了考虑冗余，可以留10%左右的字符长度。千万不能认为其为根据实际 长度来分配存储空间，而随意的分配长度，或者说干脆使用最大的字符长度。


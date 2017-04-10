MessagePack for Octave (maybe also Matlab, I dunno) 
this is a fork of Yida Zhang's msgpack-matlab so it compiles with octave; calling it this for 
search reasons.


=============
based on msgpack-c (https://github.com/msgpack/msgpack-c)

install:
 
(Octave)
    mkoctfile -mex msgpack.cc -lmsgpack
(Matlab)
    mex -O msgpack.cc -lmsgpack

API

Packer:

    >> msg = msgpack('pack', var1, var2, ...)

Add raw flag at the end of pack will enable packing all numeric type as raw (uint8)

    >> msg = msgpack('pack', var1, var2, ..., 'raw')

Unpacker:

    >> obj = msgpack('unpack', msg) 
    
return numericArray or charArray or LogicalArray if data are numeric otherwise return Cell or Struct
  
Streaming unpacker:

    >> objs = msgpack('unpacker', msg)
  
return Cell containing numericArray, charArray, Cell or Struct


#Issue

Since Matlab string is two-bytes (UTF16), following approach will not return correct string size

    >> msgpack('unpack', msgpack('pack', 'hello'))
    >> h e l l o

For correct Matlab string size, use
  
    >> msg = msgpack('pack', uint8('hello'))
    >> char(msgpack('unpack', msg))'
    >> hello

Works fine in Octave, which presumably doesn't use UTF16
var a = 15;

function foo() {
    var  a = 25;
    var result = function() {
        console.log(a);
        console.log(this.a);
    }
    result.a = 35;
    return result;
}

var bar = foo().bind({a: 45});

bar();


// 

var a = {
    lol: "wut",
    kek: function() {
        console.log(this.lol);
    }
}

a.kek();

//

var a = {
    lol: "wut",
    kek: function() {
        console.log(this.lol);
    }
}
var fn = a.kek ? a.kek : a.kek
fn();

// 

var a = {
    lol: "wut",
    kek: function() {
        console.log(this.lol);
    }
}
var fn = a.kek.bind(a).bind({lol: "asdf"});
fn();

//

var a = {
    lol: "wut",
    kek: () => {
        console.log(this.lol);
    }
}
var fn = a.kek;
fn(); // this = window
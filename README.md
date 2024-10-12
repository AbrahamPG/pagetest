# gifs.service.ts      SERVICIO PARA BUSQUEDA
import { Injectable } from '@angular/core';

@Injectable({providedIn: 'root'})
export class GifsService {

    private _tagHistory: string[]= []


    constructor() { }
    

    get tagHistory(){
        return [...this._tagHistory]
    }
    
    //transformar y mantener el ultimo digitado al principio
    private organizeHistory(tag: string){
        tag = tag.toLowerCase();

        if (this._tagHistory.includes(tag)) {
            this._tagHistory = this._tagHistory.filter( (oldTag)=> oldTag !== tag )
        }
        //mandar arriba
        this._tagHistory.unshift(tag);
        //mantener los primeros 10 "eliminados"
        this._tagHistory = this.tagHistory.splice(0,10)
    }

    async searchTag (tag:string):Promise <void>{
        if ( tag.length === 0) return;
        this.organizeHistory(tag)
        // fetch('http://api.giphy.com/v1/gifs/search?api_key=PweNxoaKYyvPQ8VoLlrZllahm1G5OILj&q=valorant&limit=10')
        // .then(resp => resp.json())                               esto es para hacer con JAVASCRIPT
        // .then(data => console.log(data))
        const params = new HttpParams()
            .set('api_key', this.apiKey)
            .set('limit', '10')
            .set('q', tag)


        this.http.get(`${this.serviceUrl}/search`, {params})
        .subscribe( resp =>{
            console.log(resp)
        })
        

    }

}


****************************************

**#search-box.component.ts**
import { Component, ElementRef, OnInit, ViewChild} from '@angular/core';
import { GifsService } from '../../services/gifs.service';

@Component({
    selector: 'gifs-search-box',
    template: `
        <div class="flex flex-col p-2 m-2 w-full">
        <h5>Buscar: </h5>
        <input type="text"
            class="border-2 border-gray-800 p-1 rounded-md flex-grow"
            placeholder="Buscar Gifs..." 
            (keyup.enter)="searchTag()"
            #txtTagInput
        >
        </div>

    `
})

export class SearchBoxComponent implements OnInit {
    constructor( private GifsService:GifsService ) { }

    ngOnInit() { }

    @ViewChild('txtTagInput')
    public tagInput!: ElementRef<HTMLInputElement>

    searchTag(){
        const newTag = this.tagInput.nativeElement.value
        this.GifsService.searchTag(newTag)
        console.log(newTag)
        this.tagInput.nativeElement.value = ''
    }

}
